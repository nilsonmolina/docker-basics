# Assignment: `Manage Multiple Containers`
- docs.docker.com and `--help` are your friend

- Run an `nginx`, a `mysql`, and an `httpd` (apache) server

- Run all of them `--detach` (or `-d`) and name them with `--name`

- nginx should listen on `80:80`, httpd on `8080:80`, mysql on `3306:3306`

- When running `mysql`, use the `--env` option (or `-e`) to pass in `MYSQL_RANDOM_ROOT_PASSWORD=yes`

- Use `docker container logs` on mysql to find the random password it created on startup

- Clean it all up with `docker container stop` and `docker container rm` (both can accept multiple names or ID's)

- Use `docker container ls` to ensure everything is correct before and after cleanup

# Solution:
1. Run the containers using the instructed configurations:
    ```docker
    $ docker container run --publish 80:80 --detach --name nginx nginx

    $ docker container run --publish 8080:80 --detach --name apache-httpd httpd

    $ docker container run --publish 3306:3306 --env MYSQL_RANDOM_ROOT_PASSWORD=yes --detach --name mysql mysql
    ```
1. Confirm the containers are running:
    ```docker
    $ docker container ls
    ```
1. Get the **Random Password** that mysql created on start-up:
    ```docker
    $ docker container logs mysql
    # GENERATED ROOT PASSWORD: fiNguf7aed4AeviushaemooChoip3zei
    ```
1. Stop the running containers:
    ```docker
    $ docker container stop nginx apache-httpd mysql
    ```
1. Confirm the containers are stopped:
    ```docker
    $ docker container ls -a
    ```
1. Remove the containers:
    ```docker
    $ docker container rm nginx apache-httpd mysql
    ```
1. Confirm the containers are removed:
    ```docker
    $ docker container ls -a
    ```
