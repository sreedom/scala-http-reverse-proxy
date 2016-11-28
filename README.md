# scala-http-reverse-proxy
An easily configurable http reverse proxy

Scala RP is an http proxying server inteded to be used as a wrapper around other HTTP services, in order to change or augment the apis they expose.

Scala RP is built on top of spray.

## Architecture

(User) --->  |RP|  <-----------> [app-server]

### Transforms
The RP is configured via pluggable transforms. Transforms are broadly classified into RequestTransforms and ResponseTransforms.

Request transforms describe how a user request on to the RP gets translated into a request to the app server, and a response transform describes the inverse flow: how a response from the server gets translated into a response to the user.

## Routes
A combination of request and response transforms are attached to a URL+method combination. This describes the entire flow of a http call. This is called a Route

By default all routes are pass-through routes, unless overridden by the configuration.

## Example
One good example is error handling. If we want to wrap an API which by default provides only http status code on error, and expose an API which gives 200OK, with error codes inside a json payload, we could write a route like this:

    errorRoute = Route('/', Request(_)->Request(_), Response(status, _) -> Response(200, body=json({"is_error": true, "status_code": status, "message": "custom error message"})))

The route composes of the url which it will match to, transform applied to the request, and the transform applied to the response.

## Configuring the RP
TBD
