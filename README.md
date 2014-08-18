Go Router
=========

Simple, compact and fast a router package for use with Go services to process HTTP requests

[![Build Status](https://travis-ci.org/takama/router.png?branch=master)](https://travis-ci.org/takama/router)
[![GoDoc](https://godoc.org/github.com/takama/router?status.svg)](https://godoc.org/github.com/takama/router)

### Examples

Simplest example (serve static route): 
```go
package main

import (
	"github.com/takama/router"
)

func Hello(c *router.Control) {
	c.Body("Hello world")
}

func main() {
	r := router.New()
	r.GET("/hello", Hello)

	// Listen and serve on 0.0.0.0:8888
	r.Listen(":8888")
}
```

Check it:
```sh
curl -i http://localhost:8888/hello/

HTTP/1.1 200 OK
Content-Type: text/plain
Date: Sun, 17 Aug 2014 13:25:50 GMT
Content-Length: 11

Hello world
```

Serve dynamic route with parameter:
```go
func main() {
	r := router.New()
	r.GET("/hello/:name", func(c *router.Control) {
		c.Body("Hello " + c.Get(":name"))
	})

	// Listen and serve on 0.0.0.0:8888
	r.Listen(":8888")
}
```

Check it:
```sh
curl -i http://localhost:8888/hello/John

HTTP/1.1 200 OK
Content-Type: text/plain
Date: Sun, 17 Aug 2014 13:25:55 GMT
Content-Length: 10

Hello John
```

Checks JSON Content-Type automatically:
```go
func main() {
	r := router.New()
	r.GET("/api/v1/settings/database/:db", func(c *router.Control) {
		data := map[string]map[string]string{
			"Database settings": {
				"database": c.Get(":db"),
				"host":     "localhost",
				"port":     "3306",
			},
		}
		c.Code(200).Body(data)
	})
	// Listen and serve on 0.0.0.0:8888
	r.Listen(":8888")
}
```

Check it:
```sh
curl -i http://localhost:8888/api/v1/settings/database/testdb

HTTP/1.1 200 OK
Content-Type: application/json
Date: Sun, 17 Aug 2014 13:25:58 GMT
Content-Length: 102

{
  "Database settings": {
    "database": "testdb",
    "host": "localhost",
    "port": "3306"
  }
}
```

## Author

[Igor Dolzhikov](https://github.com/takama)

## Contributors

All the contributors are welcome. If you would like to be the contributor please accept some rules.
- The pull requests will be accepted only in "develop" branch
- All modifications or additions should be tested
- Sorry, I'll not accept code with any dependency, only standard library

Thank you for your understanding!

## License

[BSD License](https://github.com/takama/router/blob/master/LICENSE)