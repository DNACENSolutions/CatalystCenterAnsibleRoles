# Catalyst Center Ansible Roles

This repository contains Ansible roles for managing Cisco Catalyst Center using the `cisco.catalystcenter` collection.

## Overview

This collection includes 33 roles, each corresponding to a workflow_manager module in the Catalyst Center Ansible library:

1. accesspoint
2. application_policy
3. assurance_device_health_score_settings
4. assurance_icap_settings
5. assurance_issue
6. device_configs_backup
7. device_credential
8. discovery
9. events_and_notifications
10. inventory
11. ise_radius_integration
12. lan_automation
13. network_compliance
14. network_profile_switching
15. network_profile_wireless
16. network_settings
17. path_trace
18. pnp
19. provision
20. rma
21. sda_extranet_policies
22. sda_fabric_devices
23. sda_fabric_multicast
24. sda_fabric_sites_zones
25. sda_fabric_transits
26. sda_fabric_virtual_networks
27. sda_host_port_onboarding
28. site
29. swim
30. tags
31. template
32. user_role
33. wireless_design

## Requirements

- Ansible >= 2.14
- cisco.catalystcenter collection
- Catalyst Center SDK >= 3.1.3.0.0
- Python >= 3.9

## Installation

```bash
ansible-galaxy collection install cisco.catalystcenter
```

## Usage

Each role follows standard Ansible role structure with tasks, defaults, meta, and README files.

Example playbook:

```yaml
- hosts: localhost
  roles:
    - role: site
      vars:
        catalystcenter_host: "{{ vault_catalystcenter_host }}"
        catalystcenter_username: "{{ vault_catalystcenter_username }}"
        catalystcenter_password: "{{ vault_catalystcenter_password }}"
        site_config:
          - site_type: area
            site:
              area:
                name: "USA"
                parent_name: "Global"
```

## License

GPL-3.0-or-later

## Author

Cisco Systems
