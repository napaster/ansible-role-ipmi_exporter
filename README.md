# ansible-ipmi_exporter

Role for deploy Prometheus
[ipmi_exporter](https://github.com/prometheus-community/ipmi_exporter)

## Requirements

* Ansible 9.0.0+;

Example configuration
-------------------------

```yaml
ipmi_exporter:
  # Install/upgrade exporter package, otherwise binary from Github will
  # be installed
  - install_package: 'true'
  # 'present' (do nothing if package is already installed) or 'latest' (always
  # upgrade to last version)
    package_state: 'latest'
  # version of the exporter in case of Github deployment (not relevant for
  # package deployment): bare value, like '0.8.0' or latest' - the latest
  # release from Github
    version: 'latest'
    # enable exporter service or not
    enable: 'true'
    # restart exporter service or not
    restart: 'true'
  # Exporter startup options
    options:
      # Path to FreeIPMI executables (default: rely on $PATH)
      - freeipmi_path: '/usr/bin'
      # Address to listen on for web interface and telemetry
        web_listen_address: ':9290'
      # Only log messages with the given severity or above. One of: [debug,
      # info, warn, error]
        log_level: 'info'
      # Output format of log messages. One of: [logfmt, json]
        log_format: 'logfmt'
    # The configuration of ipmi_exporter
    settings:
      modules:
        idrac:
          user: "admin"
          pass: "admin"
          driver: "LAN_2_0"
          privilege: "user"
          timeout: 10000
          collectors:
            - bmc
            - ipmi
            - chassis
            - dcmi
          collector_cmd:
            bmc: sudo
            ipmi: sudo
            dcmi: sudo
            chassis: sudo
        intel:
          user: "ADMIN"
          pass: "secret"
          driver: "LAN_2_0"
          privilege: "user"
          timeout: 10000
          collectors:
            - bmc
            - ipmi
            - chassis
          collector_cmd:
            bmc: sudo
            ipmi: sudo
            chassis: sudo
```
