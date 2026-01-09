# Ansible Role: network_profile_switching

This role manages Network Profile Switching in Cisco Catalyst Center using the `network_profile_switching_workflow_manager` module.

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
- `network_profile_switching_state`: Desired state - `merged` or `deleted` (default: `merged`)
- `network_profile_switching_config_verify`: Verify configuration after applying (default: `false`)
- `network_profile_switching_config`: List of network profile switching configurations (required)

## Dependencies

None

## Example Playbook

```yaml
- hosts: catalystcenter
  roles:
    - role: network_profile_switching
      vars:
        catalystcenter_host: "{{ vault_catalystcenter_host }}"
        catalystcenter_username: "{{ vault_catalystcenter_username }}"
        catalystcenter_password: "{{ vault_catalystcenter_password }}"
        network_profile_switching_config:
          - profile_name: "Switching-Profile-01"
```

## License

GPL-3.0-or-later

## Author Information

Cisco Systems
