# prometheus-rules

A set of prometheus alerting rules. As I'm actively using them, they'll improve over time. Feel free to add your own ideas here :)

## Idea, design, structure

This alerting rules are mainly used for several prometheus instances, so they should fit into every system. All rules are designed to be loaded even if the corresponding exporter is not present. The absence of metrics is not monitored.

The structure is kind of "historical-grown" and will maybe be refactored soon... :)

## Labels

The labels will classify the alert (from my point of view) by severity. There are some helpful labels, you may use in alertmanager or your notification tool:

### severity

The severity is kind of nagios/icinga like, so you'll get `critical` and `warning` incidents here.

### paging

You may rely on the `paging` label, which marks those alerts as important, that need your attention NOW (again, from my point of view!).

### priority

All alerts have priorities from P1 (highest) to P5 (lowest). Those priorities are aligned to the priorities of opsgenie.com, as I'm using it for notification and escalation.

## Usage

Just include them as follows:

```
git clone git@github.com:patricktoelle/prometheus-rules.git /usr/local/prometheus-rules
```

Afterwards, you may include all rules into your prometheus.yml:

```
rule_files:
- "/usr/local/prometheus-rules/generic/*.rules.yml"
- "/usr/local/prometheus-rules/services/*.rules.yml"
```

Or if you want to use a subset of rules:

**e.g. categories**:

```
rule_files:
- "/usr/local/prometheus-rules/generic/*.rules"
```

**e.g. single groups**:
```
rule_files:
- "/usr/local/prometheus-rules/generic/base.rules"
- "/usr/local/prometheus-rules/special/restic.rules"
```

## Exporters

This rules are compatible with following exporters:

* [node exporter](https://github.com/prometheus/node_exporter)
* [blackbox exporter](https://github.com/prometheus/blackbox_exporter)
* [postgresql](https://github.com/wrouesnel/postgres_exporter)
* [mysql](https://github.com/prometheus/mysqld_exporter)
* [redis](https://github.com/oliver006/redis_exporter)
* [rabbitmq](https://github.com/kbudde/rabbitmq_exporter)
* [nginx](https://github.com/hnlq715/nginx-vts-exporter)
* [haproxy](https://github.com/prometheus/haproxy_exporter)

### directly exposed

* [docker](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-metrics)

## Testing

````
find tests -name '*.test.yml' | xargs promtool test rules
```
