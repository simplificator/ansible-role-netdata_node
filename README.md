# Ansible Role: Netdata Node

[![CI](https://github.com/simplificator/ansible-role-netdata_node/workflows/CI/badge.svg?event=push)](https://github.com/simplificator/ansible-role-netdata_node/actions?query=workflow%3ACI)

This role configures an existing Netdata base installation as a client which streams its data to another Netdata server.

Its companion role is the [Netdata collector](https://github.com/simplificator/ansible-role-netdata_collector).

## Requirements

An empty Netdata installation, see dependencies.

## Role Variables

The role requires a few variables that are set:

* `netdata_registry_to_announce`: The central registry where this client should be registered. More information is available [here](https://learn.netdata.cloud/docs/agent/registry).
* `netdata_client_stream_key`: API key which will be used to identify the traffic at the server. Can be generated with `uuidgen`. The server needs to know about this UUID as well, otherwise the monitoring data of this client will be rejected.
* `netdata_client_stream_destination`: URL of the server that Netdata will sent its data.

Optional variables:

* `netdata_hostname`: Allows to overwrite the hostname for the client. Default value is "auto-detected".

Additionally, we expect that a certificate is placed at `/etc/netdata/ssl/cert.pem`. It'll be used to encrypt the traffic between node and collector.

## Dependencies

- [simplificator.netdata_installation](https://github.com/simplificator/ansible-role-netdata_installation)

## Example Playbook

```yaml
- hosts: myserver
  roles:
    - { role: simplificator.netdata_node }

  vars:
    netdata_registry_to_announce: "https://registry.example.com"
    netdata_client_stream_key: "958D31F0-C066-42CD-AE71-10D293E43F79"
    netdata_client_stream_destination: "collector.example.com:19999:SSL"
```

## Development

### Variable naming

To ensure that our Netdata roles remain compatible with each other, follow this variable naming convention:

* Role-specific variables are prefixed with the role name (`netdata_node_` in this case).
* General variables that are used in multiple roles will be prefixed with just `netdata_`.

## License

MIT / BSD
