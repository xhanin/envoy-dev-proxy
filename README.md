# Envoy Dev Proxy

Envoy Dev Proxy is intented to be used by developers who run multiple servers on their local machine.

It's typically the case for microservice development, where on the local machine each server runs on a different port.

Remembering the ports used by each server is a headache, when you need to access them with your browser or from the command line.

Plus, you often need to configure the servers address between the services, in development you usually need to specify the port of the target servers your microservice is using. Then changing those ports becomes a problem, because the port number is spread all over your config files. Or you need to use a central config file for that.

This very simple tool is here to answer this problem. It configures an Envoy proxy, exposed on a single port, that uses virtual hosts to route the traffic to the configured port on your host machine. Combined with domains resolving to 127.0.0.1, you'll be able to use meaningful names as address for all your microservices on your machine!

## Status

This tool has been hacked in a few hours - scripts may have issues, and the approach is very simple. Still it's fully functional, and has been tested both for standard http1/2 servers and gRPC services.

## Installation

Clone this repository on your local machine.

```
git clone git@github.com:xhanin/envoy-dev-proxy.git
```

_Note: providing a pre configured docker image may be done in the future_

## Configuration

You just need to add a file named `conf` at the root of the directory where you cloned this repo.
Alternatively, you can create it whereever you want, in your project sources for instance. We recommend to name it `dev-proxy-conf` in this case. 

Each line in the file must provide a single server name, and the local port associated, separated by whitespaces.

Example:
```
my-web-server                      8080
another-nice-server                8082
```

Once configured, and each time you update your conf, make sure you go in the repository directory, and run the command `./generate-envoy-config path/to/your/dev-proxy-conf` and your ready to start the proxy.

If your config file is in the default location (`conf` file in the repo directory), you can omit the path: `./generate-envoy-config`

## Running

You need docker and docker compose to run the proxy.

Launch it with `docker/compose up --detach` from the root directory.
(in case of problem, check the logs with `docker/compose logs`)

_Note: docker/compose is not a typo, there is a very compose launcher in the docker directory_

If you update the configuration, you can refresh the proxy with `docker/compose restart`

## Using

Once running, you can access the proxy on port 9999 of you local machine.

Open a browser at "http://localtest.me:9999/" and you will see the list of servers.

Each server can be accessed at `http://<server-name>.localtest.me:9999/`
For instance, http://my-web-server.localtest.me:9999/

## Contributions

Feel free to contribute if you want to improve the tool, it consists only of a few shell scripts and envoy config templates.

You can also very easily adapt it to your own needs.