# Dockerized web server to simulate delays in HTTP requests.
[![License](https://poser.pugx.org/deminy/docker-delayed-http-response/license.svg)](https://github.com/deminy/docker-delayed-http-response)

This Docker image is to simulate delays in HTTP requests. It is to help developers to test how their application behaves to a slow response from an upstream service.

# Endpoints

There are two endpoints privided:

* `/sleep/:period` - The request will be delayed for the specified period of time, in seconds.
* `/delay/:period/:url` - The request will be delayed for the specified period of time, in seconds, and then forwarded to the specified URL.

The period might takes three digits after the decimal point and must be greater than 0.001.

The web server is built using Nginx, with third-party module [ngx_echo][1] (the HTTP Echo module) and [ngx_http_lua][2] in use. The server handles concurrent requests without blocking.

# How to use it

Before making any requests, you need to start the Docker container first:

```bash
docker run --rm -d -p 80:80 -ti deminy/delayed-http-response
```

The above command will start the container in the background, and expose the port 80 to the host machine.

## Endpoint `/sleep/:period`

This endpoint always returns a 200 OK response, with string "OK" as the body. Here are some examples:

```bash
time curl -i http://127.0.0.1/sleep/1     # The request takes about 1 second to finish.
time curl -i http://127.0.0.1/sleep/1.500 # The request takes about 1.5 seconds to finish.
time curl -i http://127.0.0.1/sleep/2     # The request takes about 2 seconds to finish.
```

## Endpoint `/delay/:period/:url`

The request will be delayed for the specified period of time, in seconds, and then forwarded to the specified URL.
It only supports HTTP and HTTPS protocols. Here are some examples:

```bash
# The request is forwarded to http://example.com in 1 second.
time curl -svfL http://127.0.0.1/delay/1/http://example.com

# The request is forwarded to https://nginx.org/en/ in 2 seconds.
time curl -svfL http://127.0.0.1/delay/2/https://nginx.org/en/
```

# Alternatives

* [Deelay][3]: Inline delay proxy for http resources. Written in Node.js.
* [http-delayed-response][4]: A fast and easy way to delay a response until results are available. Written in Node.js.

[1]: https://github.com/openresty/echo-nginx-module
[2]: https://github.com/openresty/lua-nginx-module
[3]: https://github.com/biesiad/deelay
[4]: https://github.com/extrabacon/http-delayed-response
