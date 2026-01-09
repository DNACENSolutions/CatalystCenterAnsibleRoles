# Ansible Role: assurance_icap_settings

This role manages Assurance ICAP Settings in Cisco Catalyst Center using the `assurance_icap_settings_workflow_manager` module.

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
- `assurance_icap_settings_state`: Desired state - `merged` or `deleted` (default: `merged`)
- `assurance_icap_settings_config_verify`: Verify configuration after applying (default: `false`)
- `assurance_icap_settings_config`: List of assurance ICAP settings configurations (required)

## Dependencies

None

## Example Playbook

```yaml
- hosts: catalystcenter
  roles:
    - role: assurance_icap_settings
      vars:
        catalystcenter_host: "{{ vault_catalystcenter_host }}"
        catalystcenter_username: "{{ vault_catalystcenter_username }}"
        catalystcenter_password: "{{ vault_catalystcenter_password }}"
        assurance_icap_settings_config:
          - icap_server: "icap-server-01"
```

## License

GPL-3.0-or-later

## Author Information

Cisco Systems
