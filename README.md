# Go RESTful API

A Restful API based

Thanks to
github.com/qiangxue/golang-restful-starter-kit


## Getting Started

If this is your first time encountering Go, please follow [the instructions](https://golang.org/doc/install) to
install Go on your computer. The kit requires Go 1.5 or above.

After installing Go, run the following commands to download and install this starter kit:

```shell
# install the starter kit
go get github.com/qiangxue/golang-restful-starter-kit

# install glide (a vendoring and dependency management tool), if you don't have it yet
go get -u github.com/Masterminds/glide

# fetch the dependent packages
cd $GOPATH/qiangxue/golang-restful-starter-kit
make depends   # or "glide up"
```

Next, create a PostgreSQL database named `go_restful` and execute the SQL statements given in the file `testdata/db.sql`.
The starter kit uses the following default database connection information:
* server address: `127.0.0.1` (local machine)
* server port: `5432`
* database name: `go_restful`
* username: `postgres`
* password: `postgres`

If your connection is different from the above, you may modify the configuration file `config/app.yaml`, or
define an environment variable named `RESTFUL_DSN` like the following:

```
postgres://<username>:<password>@<server-address>:<server-port>/<db-name>
```

For more details about specifying a PostgreSQL DSN, please refer to [the documentation](https://godoc.org/github.com/lib/pq).

Now you can build and run the application by running the following command under the
`$GOPATH/qiangxue/golang-restful-starter-kit` directory:

```shell
go run server.go
```

or simply the following if you have the `make` tool:

```shell
make
```

The application runs as an HTTP server at port 8080. It provides the following RESTful endpoints:

* `GET /ping`: a ping service mainly provided for health check purpose
* `POST /v1/auth`: authenticate a user
* `GET /v1/artists`: returns a paginated list of the artists
* `GET /v1/artists/:id`: returns the detailed information of an artist
* `POST /v1/artists`: creates a new artist
* `PUT /v1/artists/:id`: updates an existing artist
* `DELETE /v1/artists/:id`: deletes an artist

For example, if you access the URL `http://localhost:8080/ping` in a browser, you should see the browser
displays something like `OK v0.1#bc41dce`.

If you have `cURL` or some API client tools (e.g. Postman), you may try the following more complex scenarios:

```shell
# authenticate the user via: POST /v1/auth
curl -X POST -H "Content-Type: application/json" -d '{"username": "demo", "password": "pass"}' http://localhost:8080/v1/auth
# should return a JWT token like: {"token":"...JWT token here..."}

# with the above JWT token, access the artist resources, such as: GET /v1/artists
curl -X GET -H "Authorization: Bearer ...JWT token here..." http://localhost:8080/v1/artists
# should return a list of artist records in the JSON format
```