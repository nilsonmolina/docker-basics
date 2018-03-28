# Assignment: DNS Round Robin Test
**Objective:**  
*DNS Round Robin is the concept that you can have two different hosts with DNS aliases that respond to the same DNS name. Ever since Docker Engine 1.11, we can have multiple containers on a created network respond to the same DNS address. It can sometimes be referred to as a "Poor Mans Load Balancer".*

**Requirements:**
- Create a new virtual network using the default bridge driver.

- Create two containers from `elasticsearch:2` image.

- Research and use `--network-alias search` when creating them to give them an additional DNS name to respond to.

- Run `alpine nslookup search` with `--network` to see the two containers list for the same DNS name

- Run `centos curl -s search:9200` with `--network` multiple times until you see both "name" fields show.

# Solution
1. Learn about the `--network-alias` option:
    ```
    $ docker container run --help
    ```

1. Create a new virtual network using the default bridge driver:
    ```bash
    $ docker network create round_robin
    ```

1. Create two containers from `elasticsearch:2` image using the newly created network and with the same network alias:
    ```bash
    $ docker container run -d --network round_robin --network-alias search elasticsearch:2 

    $ docker container run -d --network round_robin --network-alias search elasticsearch:2 
    ```
    **_*Note:_** *We do not assign a port, because all communication between the containers will be done within the same network. No need to communicate w/ the outside world (wide web)!*

1. Confirm that both containers are running:
    ```bash
    $ docker container ls
    ```

1. To test our round robin, lets spin up a container that will run an `nslookup search`, close, and remove itself:
    ```bash
    $ docker container run --rm --network round_robin alpine nslookup search

    Name:      search
    Address 1: 172.19.0.3 search.round_robin
    Address 2: 172.19.0.2 search.round_robin
    ```

1. Calling the DNS multiple times will have will have the containers alternate who responds.  This is usually random, you can call the following command multiple times to see which server responds.
    ```bash
    $ docker container run --rm --network round_robin centos curl -s search:9200
    {
    "name" : "Ankhi",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "r78hmDNISs6XYbELE2cxMg",
    "version" : {
        "number" : "2.4.6",
        "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
        "build_timestamp" : "2017-07-18T12:17:44Z",
        "build_snapshot" : false,
        "lucene_version" : "5.5.4"
    },
    "tagline" : "You Know, for Search"
    }

    $ docker container run --rm --network round_robin centos curl -s search:9200
    {
    "name" : "Wade Wilson",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "rrPGyyiCRjy41HiIlBiBTA",
    "version" : {
        "number" : "2.4.6",
        "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
        "build_timestamp" : "2017-07-18T12:17:44Z",
        "build_snapshot" : false,
        "lucene_version" : "5.5.4"
    },
    "tagline" : "You Know, for Search"
    }

    ```
    **_Note:_** *Run this command multiple times to see the different containers.*