# Ansible Role: wireless_design

This role manages Wireless Design in Cisco Catalyst Center using the `wireless_design_workflow_manager` module.

## Requirements

- `cisco.catalystcenter` collection installed
- Catalyst Center SDK >= 3.1.3.0.0
- Python >= 3.9

## Role Variables

### Connection Variables
- `catalystcenter_host`: Catalyst Center hostname or IP address (required)
- `catalystcenter_username`: Username for authentication (required)
- `catalystcenter_password`: Password for authentication (required)

### Role-Specific Variables
- `wireless_design_state`: Desired state - `merged` or `deleted` (default: `merged`)
- `wireless_design_config_verify`: Verify configuration after applying (default: `false`)
- `wireless_design_config`: List of wireless design configurations (required)

## License

GPL-3.0-or-later

## Author Information

Cisco Systems
