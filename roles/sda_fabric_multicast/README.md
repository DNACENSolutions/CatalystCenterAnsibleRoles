# Ansible Role: sda_fabric_multicast

This role manages SDA Fabric Multicast in Cisco Catalyst Center using the `sda_fabric_multicast_workflow_manager` module.

## Requirements

- `cisco.catalystcenter` collection installed
- Catalyst Center SDK >= 3.1.3.0.0
- Python >= 3.9

## Role Variables

### Connection Variables
- `catalystcenter_host`: Catalyst Center hostname or IP address (required)
- `catalystcenter_username`: Username for authentication (required)
- `catalystcenter_password`: Password for authentication (required)
- `catalystcenter_verify`: SSL certificate verification (default: `false`)
- `catalystcenter_port`: API port (default: `443`)
- `catalystcenter_version`: Catalyst Center version (default: `2.3.7.6`)
- `catalystcenter_debug`: Enable debug mode (default: `false`)
- `catalystcenter_log_level`: Logging level (default: `INFO`)
- `catalystcenter_log`: Enable logging (default: `false`)

### Role-Specific Variables
- `sda_fabric_multicast_state`: Desired state - `merged` or `deleted` (default: `merged`)
- `sda_fabric_multicast_config_verify`: Verify configuration after applying (default: `false`)
- `sda_fabric_multicast_config`: List of SDA fabric multicast configurations (required)

## Dependencies

None

## Example Playbook

```yaml
- hosts: catalystcenter
  roles:
    - role: sda_fabric_multicast
      vars:
        catalystcenter_host: "{{ vault_catalystcenter_host }}"
        catalystcenter_username: "{{ vault_catalystcenter_username }}"
        catalystcenter_password: "{{ vault_catalystcenter_password }}"
        sda_fabric_multicast_config:
          - fabric_site_name: "Global/USA/Building1"
```

## License

GPL-3.0-or-later

## Author Information

Cisco Systems
