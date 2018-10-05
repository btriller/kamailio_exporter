# Kamailio Exporter for Prometheus
[![CircleCI](https://circleci.com/gh/florentchauveau/kamailio_exporter.svg?style=shield)](https://circleci.com/gh/florentchauveau/kamailio_exporter)

A [Kamailio](https://www.kamailio.org/) exporter for Prometheus.

It communicates with Kamailio using native [BINRPC](http://kamailio.org/docs/modules/stable/modules/ctl.html) via the `ctl` module. 

BINRPC is implemented in library https://github.com/florentchauveau/go-kamailio-binrpc.

## Gettings Started

To run it:
```bash
./kamailio_exporter [flags]
```

Help on flags:
```
./kamailio_exporter --help

Flags:
      --help                 Show context-sensitive help (also try --help-long
                             and --help-man).
  -l, --web.listen-address=":9494"
                             Address to listen on for web interface and
                             telemetry.
      --web.telemetry-path="/metrics"
                             Path under which to expose metrics.
  -u, --kamailio.scrape-uri="unix:/var/run/kamailio/kamailio_ctl"
                             URI on which to scrape kamailio. E.g.
                             "unix:/var/run/kamailio/kamailio_ctl" or
                             "tcp://localhost:2049"
  -m, --kamailio.methods="tm.stats,sl.stats,core.shmmem,core.uptime"
                             Comma-separated list of methods to call. E.g.
                             "tm.stats,sl.stats". Implemented:
                             tm.stats,sl.stats,core.shmmem,core.uptime,dispatcher.list
  -t, --kamailio.timeout=5s  Timeout for trying to get stats from kamailio.
  ```

## Usage

The [CTL](http://kamailio.org/docs/modules/stable/modules/ctl.html) module must be loaded by the Kamailio instance. If you are using `kamcmd` (and you probably are), the module is already loaded.

By default (if no parameters are changed in the config file), the `ctl` module exposes a Unix stream socket: `/var/run/kamailio/kamailio_ctl`. If you change it, specify the scrape URI with the `--kamailio.scrape-uri` flag. Example:

```
./kamailio_exporter -u "tcp://localhost:2049"
```

## Metrics

By default, the exporter will try to fetch values from the following commands:

- `tm.stats` (requires the [TM](http://kamailio.org/docs/modules/stable/modules/tm.html) module)
- `sl.stats` (requires the [SL](http://kamailio.org/docs/modules/stable/modules/sl.html) module)
- `core.shmmem`
- `core.uptime`

If you are using the [DISPATCHER](http://kamailio.org/docs/modules/stable/modules/dispatcher.html) module, you can enable `dispatcher.list` as well:

```bash
./kamailio_exporter -m "tm.stats,sl.stats,core.shmmem,core.uptime,dispatcher.list"
```

List of exposed metrics:
```
# HELP kamailio_core_shmmem_fragments fragments shared memory
# TYPE kamailio_core_shmmem_fragments gauge
# HELP kamailio_core_shmmem_free free shared memory
# TYPE kamailio_core_shmmem_free gauge
# HELP kamailio_core_shmmem_max_used max_used shared memory
# TYPE kamailio_core_shmmem_max_used gauge
# HELP kamailio_core_shmmem_real_used real_used shared memory
# TYPE kamailio_core_shmmem_real_used gauge
# HELP kamailio_core_shmmem_total total shared memory
# TYPE kamailio_core_shmmem_total gauge
# HELP kamailio_core_shmmem_used used shared memory
# TYPE kamailio_core_shmmem_used gauge
# HELP kamailio_core_uptime_uptime_total uptime in seconds
# TYPE kamailio_core_uptime_uptime_total counter
# HELP kamailio_dispatcher_list_target target status
# TYPE kamailio_dispatcher_list_target gauge
# HELP kamailio_exporter_failed_scrapes Number of failed kamailio scrapes.
# TYPE kamailio_exporter_failed_scrapes counter
# HELP kamailio_exporter_total_scrapes Current total kamailio scrapes.
# TYPE kamailio_exporter_total_scrapes counter
# HELP kamailio_sl_stats_200_total 200 replies counter
# TYPE kamailio_sl_stats_200_total counter
# HELP kamailio_sl_stats_202_total 202 replies counter
# TYPE kamailio_sl_stats_202_total counter
# HELP kamailio_sl_stats_2xx_total 2xx replies counter
# TYPE kamailio_sl_stats_2xx_total counter
# HELP kamailio_sl_stats_300_total 300 replies counter
# TYPE kamailio_sl_stats_300_total counter
# HELP kamailio_sl_stats_301_total 301 replies counter
# TYPE kamailio_sl_stats_301_total counter
# HELP kamailio_sl_stats_302_total 302 replies counter
# TYPE kamailio_sl_stats_302_total counter
# HELP kamailio_sl_stats_400_total 400 replies counter
# TYPE kamailio_sl_stats_400_total counter
# HELP kamailio_sl_stats_401_total 401 replies counter
# TYPE kamailio_sl_stats_401_total counter
# HELP kamailio_sl_stats_403_total 403 replies counter
# TYPE kamailio_sl_stats_403_total counter
# HELP kamailio_sl_stats_404_total 404 replies counter
# TYPE kamailio_sl_stats_404_total counter
# HELP kamailio_sl_stats_407_total 407 replies counter
# TYPE kamailio_sl_stats_407_total counter
# HELP kamailio_sl_stats_408_total 408 replies counter
# TYPE kamailio_sl_stats_408_total counter
# HELP kamailio_sl_stats_483_total 483 replies counter
# TYPE kamailio_sl_stats_483_total counter
# HELP kamailio_sl_stats_4xx_total 4xx replies counter
# TYPE kamailio_sl_stats_4xx_total counter
# HELP kamailio_sl_stats_500_total 500 replies counter
# TYPE kamailio_sl_stats_500_total counter
# HELP kamailio_sl_stats_5xx_total 5xx replies counter
# TYPE kamailio_sl_stats_5xx_total counter
# HELP kamailio_sl_stats_6xx_total 6xx replies counter
# TYPE kamailio_sl_stats_6xx_total counter
# HELP kamailio_sl_stats_xxx_total xxx replies counter
# TYPE kamailio_sl_stats_xxx_total counter
# HELP kamailio_tm_stats_2xx_total 2xx transactions
# TYPE kamailio_tm_stats_2xx_total counter
# HELP kamailio_tm_stats_3xx_total 3xx transactions
# TYPE kamailio_tm_stats_3xx_total counter
# HELP kamailio_tm_stats_4xx_total 4xx transactions
# TYPE kamailio_tm_stats_4xx_total counter
# HELP kamailio_tm_stats_5xx_total 5xx transactions
# TYPE kamailio_tm_stats_5xx_total counter
# HELP kamailio_tm_stats_6xx_total 6xx transactions
# TYPE kamailio_tm_stats_6xx_total counter
# HELP kamailio_tm_stats_created_total created transactions
# TYPE kamailio_tm_stats_created_total counter
# HELP kamailio_tm_stats_current current transactions
# TYPE kamailio_tm_stats_current gauge
# HELP kamailio_tm_stats_delayed_free_total delayed_free transactions
# TYPE kamailio_tm_stats_delayed_free_total counter
# HELP kamailio_tm_stats_freed_total freed transactions
# TYPE kamailio_tm_stats_freed_total counter
# HELP kamailio_tm_stats_rpl_generated_total rpl_generated transactions
# TYPE kamailio_tm_stats_rpl_generated_total counter
# HELP kamailio_tm_stats_rpl_received_total rpl_received transactions
# TYPE kamailio_tm_stats_rpl_received_total counter
# HELP kamailio_tm_stats_rpl_sent_total rpl_sent transactions
# TYPE kamailio_tm_stats_rpl_sent_total counter
# HELP kamailio_tm_stats_total_local_total total_local transactions
# TYPE kamailio_tm_stats_total_local_total counter
# HELP kamailio_tm_stats_total_total total transactions
# TYPE kamailio_tm_stats_total_total counter
# HELP kamailio_up Was the last scrape successful.
# TYPE kamailio_up gauge
```

## Contributing

Feel free to send pull requests.
