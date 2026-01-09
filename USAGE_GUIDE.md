# Catalyst Center Ansible Roles - Usage Guide

## Quick Start

### 1. Install Requirements

```bash
# Install the Catalyst Center collection
ansible-galaxy collection install -r requirements.yml

# Install Python dependencies
pip install catalystcentersdk>=3.1.3.0.0
```

### 2. Configure Credentials

Create a vault file to store sensitive credentials:

```bash
ansible-vault create group_vars/all/vault.yml
```

Add the following content:

```yaml
vault_catalystcenter_host: "catalystcenter.example.com"
vault_catalystcenter_username: "admin"
vault_catalystcenter_password: "your_password"
vault_device_password: "device_password"
vault_device_enable_password: "enable_password"
```

### 3. Run a Playbook

```bash
ansible-playbook example_playbook.yml --ask-vault-pass
```

## Role Structure

Each role follows the standard Ansible role structure:

```
role_name/
├── tasks/
│   └── main.yml          # Main task file calling the workflow_manager module
├── defaults/
│   └── main.yml          # Default variables
├── meta/
│   └── main.yml          # Role metadata
└── README.md             # Role documentation
```

## Common Variables

All roles share these common connection variables:

- `catalystcenter_host`: Catalyst Center hostname or IP
- `catalystcenter_username`: Authentication username
- `catalystcenter_password`: Authentication password
- `catalystcenter_verify`: SSL verification (default: false)
- `catalystcenter_port`: API port (default: 443)
- `catalystcenter_version`: Catalyst Center version (default: 2.3.7.6)
- `catalystcenter_debug`: Enable debug mode (default: false)
- `catalystcenter_log_level`: Logging level (default: INFO)
- `catalystcenter_log`: Enable logging (default: false)

## Role-Specific Variables

Each role has specific variables:

- `<role_name>_state`: Desired state (merged/deleted)
- `<role_name>_config_verify`: Verify configuration after applying
- `<role_name>_config`: List of configurations

## Usage Examples

### Site Management

```yaml
- hosts: localhost
  roles:
    - role: site
      vars:
        site_config:
          - site_type: area
            site:
              area:
                name: "USA"
                parent_name: "Global"
          - site_type: building
            site:
              building:
                name: "Building1"
                parent_name: "Global/USA"
                address: "123 Main St, San Jose, CA"
                latitude: 37.338
                longitude: -121.832
          - site_type: floor
            site:
              floor:
                name: "Floor1"
                parent_name: "Global/USA/Building1"
                rf_model: "Cubes And Walled Offices"
                length: 100.0
                width: 100.0
                height: 10.0
```

### Device Discovery

```yaml
- hosts: localhost
  roles:
    - role: discovery
      vars:
        discovery_config:
          - discovery_name: "Network-Discovery"
            discovery_type: "Range"
            ip_address_list: "10.0.0.0/24"
            protocol_order: "ssh"
            timeout: 5
            retry_count: 3
```

### Device Provisioning

```yaml
- hosts: localhost
  roles:
    - role: provision
      vars:
        provision_config:
          - device_ip: "10.0.0.1"
            site_name: "Global/USA/Building1/Floor1"
```

### SDA Fabric Configuration

```yaml
- hosts: localhost
  roles:
    - role: sda_fabric_sites_zones
      vars:
        sda_fabric_sites_zones_config:
          - fabric_site_name: "Global/USA/Building1"
            fabric_type: "FABRIC_SITE"
    
    - role: sda_fabric_devices
      vars:
        sda_fabric_devices_config:
          - device_ip: "10.0.0.1"
            fabric_site_name: "Global/USA/Building1"
            device_roles:
              - "CONTROL_PLANE_NODE"
              - "BORDER_NODE"
```

### Template Management

```yaml
- hosts: localhost
  roles:
    - role: template
      vars:
        template_config:
          - template_name: "Interface-Config"
            project_name: "Onboarding"
            template_content: |
              interface {{ interface }}
                description {{ description }}
                no shutdown
            template_params:
              - parameter_name: "interface"
                data_type: "STRING"
                required: true
              - parameter_name: "description"
                data_type: "STRING"
                required: true
```

### Software Image Management (SWIM)

```yaml
- hosts: localhost
  roles:
    - role: swim
      vars:
        swim_config:
          - image_name: "cat9k_iosxe.17.09.04a.SPA.bin"
            site_name: "Global/USA/Building1"
            device_family: "Cisco Catalyst 9300 Series Switches"
```

### Wireless Configuration

```yaml
- hosts: localhost
  roles:
    - role: wireless_design
      vars:
        wireless_design_config:
          - ssid_name: "Corporate-WiFi"
            security_level: "wpa2_enterprise"
            passphrase: "{{ vault_wifi_passphrase }}"
```

## Best Practices

### 1. Use Ansible Vault for Credentials

Always store sensitive information in Ansible Vault:

```bash
ansible-vault encrypt_string 'my_password' --name 'vault_catalystcenter_password'
```

### 2. Organize Variables

Use group_vars and host_vars for better organization:

```
group_vars/
  all/
    common.yml
    vault.yml
  catalystcenter/
    settings.yml
```

### 3. Enable Configuration Verification

Set `config_verify: true` to verify configurations after applying:

```yaml
site_config_verify: true
```

### 4. Use Tags for Selective Execution

Tag your tasks for selective execution:

```yaml
- hosts: localhost
  roles:
    - role: site
      tags: ['site', 'infrastructure']
    - role: provision
      tags: ['provision', 'devices']
```

Run specific tags:

```bash
ansible-playbook playbook.yml --tags site
```

### 5. Idempotency

All roles are designed to be idempotent. Running the same playbook multiple times will not create duplicate resources.

### 6. Error Handling

Use blocks for error handling:

```yaml
- block:
    - name: Configure Sites
      ansible.builtin.include_role:
        name: site
      vars:
        site_config: "{{ site_configuration }}"
  rescue:
    - name: Handle Error
      ansible.builtin.debug:
        msg: "Site configuration failed: {{ ansible_failed_result }}"
```

## Troubleshooting

### Enable Debug Mode

```yaml
catalystcenter_debug: true
catalystcenter_log: true
catalystcenter_log_level: "DEBUG"
```

### Check Module Output

```yaml
- name: Configure Sites
  ansible.builtin.include_role:
    name: site
  register: site_result

- name: Display Result
  ansible.builtin.debug:
    var: site_result
```

### Verify API Connectivity

```bash
curl -k https://catalystcenter.example.com/dna/system/api/v1/auth/token \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"password"}'
```

## Available Roles

| Role Name | Description |
|-----------|-------------|
| accesspoint | Manage access points |
| application_policy | Manage application policies |
| assurance_device_health_score_settings | Configure device health score settings |
| assurance_icap_settings | Configure ICAP settings |
| assurance_issue | Manage assurance issues |
| device_configs_backup | Backup device configurations |
| device_credential | Manage device credentials |
| discovery | Discover network devices |
| events_and_notifications | Configure events and notifications |
| inventory | Manage device inventory |
| ise_radius_integration | Configure ISE RADIUS integration |
| lan_automation | Manage LAN automation |
| network_compliance | Check network compliance |
| network_profile_switching | Manage switching network profiles |
| network_profile_wireless | Manage wireless network profiles |
| network_settings | Configure network settings |
| path_trace | Perform path trace operations |
| pnp | Manage Plug and Play |
| provision | Provision devices |
| rma | Manage RMA operations |
| sda_extranet_policies | Configure SDA extranet policies |
| sda_fabric_devices | Manage SDA fabric devices |
| sda_fabric_multicast | Configure SDA fabric multicast |
| sda_fabric_sites_zones | Manage SDA fabric sites and zones |
| sda_fabric_transits | Configure SDA fabric transits |
| sda_fabric_virtual_networks | Manage SDA virtual networks |
| sda_host_port_onboarding | Onboard SDA host ports |
| site | Manage site hierarchy |
| swim | Manage software images |
| tags | Manage tags |
| template | Manage configuration templates |
| user_role | Manage user roles |
| wireless_design | Configure wireless design |

## Support

For issues and questions:
- Cisco Catalyst Center Ansible Collection: https://github.com/cisco-en-programmability/catalystcenter-ansible
- Cisco DevNet: https://developer.cisco.com/
