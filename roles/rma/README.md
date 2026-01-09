# Ansible Role: rma

This role manages RMA (Return Merchandise Authorization) in Cisco Catalyst Center using the `rma_workflow_manager` module.

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
- `rma_state`: Desired state - `merged` or `deleted` (default: `merged`)
- `rma_config_verify`: Verify configuration after applying (default: `false`)
- `rma_config`: List of RMA configurations (required)

## Dependencies

None

## Example Playbook

```yaml
- hosts: catalystcenter
  roles:
    - role: rma
      vars:
        catalystcenter_host: "{{ vault_catalystcenter_host }}"
        catalystcenter_username: "{{ vault_catalystcenter_username }}"
        catalystcenter_password: "{{ vault_catalystcenter_password }}"
        rma_config:
          - faulty_device_serial_number: "FCW1234ABCD"
            replacement_device_serial_number: "FCW5678EFGH"
```

## License

GPL-3.0-or-later

## Author Information

Cisco Systems
