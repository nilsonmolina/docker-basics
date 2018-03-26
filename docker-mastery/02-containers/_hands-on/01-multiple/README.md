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

