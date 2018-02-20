# php-fpm_exporter

[![CircleCI](https://circleci.com/gh/hipages/php-fpm_exporter.svg?style=svg)](https://circleci.com/gh/hipages/php-fpm_exporter)
[![Go Report Card](https://goreportcard.com/badge/github.com/hipages/php-fpm_exporter)](https://goreportcard.com/report/github.com/hipages/php-fpm_exporter)
[![GoDoc](https://godoc.org/github.com/hipages/php-fpm_exporter?status.svg)](https://godoc.org/github.com/hipages/php-fpm_exporter)
[![Inline docs](http://inch-ci.org/github/hipages/php-fpm_exporter.svg?branch=master)](http://inch-ci.org/github/hipages/php-fpm_exporter)
[![Maintainability](https://api.codeclimate.com/v1/badges/52f9e1f0388e8aa38bfe/maintainability)](https://codeclimate.com/github/hipages/php-fpm_exporter/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/52f9e1f0388e8aa38bfe/test_coverage)](https://codeclimate.com/github/hipages/php-fpm_exporter/test_coverage)

A [prometheus](https://prometheus.io/) exporter for PHP-FPM.
The exporter connects directly to PHP-FPM and exports the metrics via HTTP.

A webserver such as NGINX or Apache is **NOT** needed!

## Features

* Export single or multiple pools
* Export to CLI as text or JSON
* Connects directly to PHP-FPM via TCP or Socket
* Maps environment variables to CLI options

## Usage

`php-fpm_exporter` is released as [binary](https://github.com/hipages/php-fpm_exporter/releases) and [docker](https://hub.docker.com/r/hipages/php-fpm_exporter/) image.
It uses sensible defaults which usually avoids the need to use command parameters or environment variables.

`php-fpm_exporter` supports 2 commands, `get` and `server`.
The `get` command allows to retrieve information from PHP-FPM without running as a server and exposing an endpoint.
The `server` command runs the server required for prometheus to retrieve the statistics.

### Options and defaults

| Option                 | Description                                           | Environment variable         | Default value   |
|------------------------|-------------------------------------------------------|------------------------------|-----------------|
| `--web.listen-address` | Address on which to expose metrics and web interface. | `PHP_FPM_WEB_LISTEN_ADDRESS` | [`:9253`](https://github.com/prometheus/prometheus/wiki/Default-port-allocations)         |
| `--web.telemetry-path` | Path under which to expose metrics.                   | `PHP_FPM_WEB_TELEMETRY_PATH` | `/metrics`      |
| `--phpfpm.scrape-uri`  | FastCGI address, e.g. unix:///tmp/php.sock;/status or tcp://127.0.0.1:9000/status | `PHP_FPM_SCRAPE_URI` | `tcp://127.0.0.1:9000/status` |
| `--log.level`          | Only log messages with the given severity or above. Valid levels: [debug, info, warn, error, fatal] (default "error") | PHP_FPM_LOG_LEVEL | info |

### CLI Examples

* Retrieve information from PHP-FPM running on `127.0.0.1:9000` with status endpoint being `/status`
  ```
  php-fpm_exporter get
  ```

* Retrieve information from PHP-FPM running on `127.0.0.1:9000` and `127.0.0.1:9001`
  ```
  php-fpm_exporter get --phpfpm.scrape-uri tcp://127.0.0.1:9000/status,tcp://127.0.0.1:9001/status
  ```

* Run as server with 2 pools:
  ```
  php-fpm_exporter server --phpfpm.scrape-uri tcp://127.0.0.1:9000/status,tcp://127.0.0.1:9001/status
  ```

### Docker Examples

* Run docker manually
  ```
  docker pull hipages/php-fpm_exporter
  docker run -it --rm -e PHP_FPM_SCRAPE_URI="tcp://127.0.0.1:9000/status,tcp://127.0.0.1:9001/status" hipages/php-fpm_exporter
  ```

* Run the docker-compose example
  ```
  git clone git@github.com:hipages/php-fpm_exporter.git
  cd php-fpm_exporter/test
  docker-compose -p php-fpm_exporter up
  ```
  You can now access the following links:

  * Prometheus: http://127.0.0.1:9090/
  * php-fpm_exporter metrics: http://127.0.0.1:9253/metrics

  [![asciicast](https://asciinema.org/a/1msR8nqAsFdHzROosUb7PiHvf.png)](https://asciinema.org/a/1msR8nqAsFdHzROosUb7PiHvf)

## Metrics collected

```
# HELP phpfpm_accepted_connections The number of requests accepted by the pool.
# TYPE phpfpm_accepted_connections counter
# HELP phpfpm_active_processes The number of active processes.
# TYPE phpfpm_active_processes gauge
# HELP phpfpm_idle_processes The number of idle processes.
# TYPE phpfpm_idle_processes gauge
# HELP phpfpm_listen_queue The number of requests in the queue of pending connections.
# TYPE phpfpm_listen_queue gauge
# HELP phpfpm_listen_queue_length The size of the socket queue of pending connections.
# TYPE phpfpm_listen_queue_length gauge
# HELP phpfpm_max_active_processes The maximum number of active processes since FPM has started.
# TYPE phpfpm_max_active_processes counter
# HELP phpfpm_max_children_reached The number of times, the process limit has been reached, when pm tries to start more children (works only for pm 'dynamic' and 'ondemand').
# TYPE phpfpm_max_children_reached counter
# HELP phpfpm_max_listen_queue The maximum number of requests in the queue of pending connections since FPM has started.
# TYPE phpfpm_max_listen_queue counter
# HELP phpfpm_slow_requests The number of requests that exceeded your 'request_slowlog_timeout' value.
# TYPE phpfpm_slow_requests counter
# HELP phpfpm_start_since The number of seconds since FPM has started.
# TYPE phpfpm_start_since counter
# HELP phpfpm_total_processes The number of idle + active processes.
# TYPE phpfpm_total_processes gauge
```

## Contributing

Contributions are greatly appreciated.
The maintainers actively manage the issues list, and try to highlight issues suitable for newcomers.
The project follows the typical GitHub pull request model.
See " [How to Contribute to Open Source](https://opensource.guide/how-to-contribute/) " for more details.
Before starting any work, please either comment on an existing issue, or file a new one.

## Alternatives

* [bakins/php-fpm-exporter](https://github.com/bakins/php-fpm-exporter)
* [peakgames/php-fpm-prometheus](https://github.com/peakgames/php-fpm-prometheus)
* [craigmj/phpfpm_exporter](https://github.com/craigmj/phpfpm_exporter)