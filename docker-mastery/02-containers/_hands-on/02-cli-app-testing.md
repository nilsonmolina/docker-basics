# Assignment: CLI App Testing
**Objective:**  
*Use different Linux distro containers to check `curl` cli tool version.*

**Requirements:**
- Use two different terminal windows to start bash in both `centos:7` and `ubuntu:14.04`, using `-it`

- Learn the `docker container --rm` option so you can save cleanup

- Ensure `curl` is installed and on latest version for that distro
    - ubuntu: `apt-get update && apt-get install curl`
    - centos: `yum update curl`

- Check `curl --version`

# Solution:
1. Learn how the `--rm` option works:
    ```
    $ docker container run --help
    ```

1. Spin up a centos and ubuntu container that starts in bash and will remove itself once closed. - *(Run each command on it's own terminal window)*
    ```bash
    # on centos window
    $ docker container run --rm -it centos:7 bash
    ```
    ```bash
    # on ubuntu window
    $ docker container run --rm -it ubuntu:14.04 bash
    ```

1. Ensure curl is installed and on latest version for that distro:
    ```bash
    # on centos window
    $ yum update curl
    ```
    ```bash
    # on ubuntu window
    $ apt-get update && apt-get install curl
    ```

1. Check the currently installed version of curl:
    ```bash
    # on centos window
    $ curl --version

    curl 7.29.0 (x86_64-redhat-linux-gnu) libcurl/7.29.0 NSS/3.28.4 zlib/1.2.7 libidn/1.28 libssh2/1.4.3
    Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtsp scp sftp smtp smtps telnet tftp
    Features: AsynchDNS GSS-Negotiate IDN IPv6 Largefile NTLM NTLM_WB SSL libz unix-sockets
    ```
    ```bash
    # on ubuntu window
    $ curl --version

    curl 7.35.0 (x86_64-pc-linux-gnu) libcurl/7.35.0 OpenSSL/1.0.1f zlib/1.2.8 libidn/1.28 librtmp/2.3
    Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smtp smtps telnet tftp
    Features: AsynchDNS GSS-Negotiate IDN IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP
    ```

1. Close bash, which will also stop the container, and remove it, because of the flags we used when starting them up:
    ```bash
    # on centos window
    $ exit
    ```
    ```bash
    # on ubuntu window
    $ exit
    ```

1. Confirm that the containers have been removed:
    ```bash
    $ docker container ls -a
    ```