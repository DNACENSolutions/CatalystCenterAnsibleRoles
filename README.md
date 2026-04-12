# Catalyst Center Ansible Roles

This repository contains Ansible roles for managing Cisco Catalyst Center using the `cisco.catalystcenter` collection.

## Overview

This role set now includes **69 roles** split across two families:

- **39 workflow-manager roles** that map directly to `*_workflow_manager` modules.
- **30 config-generator roles** that wrap `*_playbook_config_generator` modules and use the `_config_generator` suffix for unambiguous role names.

### Workflow-manager roles

- `accesspoint`
- `accesspoint_location`
- `application_policy`
- `assurance_device_health_score_settings`
- `assurance_icap_settings`
- `assurance_issue`
- `backup_and_restore`
- `device_configs_backup`
- `device_credential`
- `discovery`
- `events_and_notifications`
- `fabric_devices_info`
- `inventory`
- `ise_radius_integration`
- `lan_automation`
- `network_compliance`
- `network_devices_info`
- `network_profile_switching`
- `network_profile_wireless`
- `network_settings`
- `path_trace`
- `pnp`
- `provision`
- `reports`
- `rma`
- `sda_extranet_policies`
- `sda_fabric_devices`
- `sda_fabric_multicast`
- `sda_fabric_sites_zones`
- `sda_fabric_transits`
- `sda_fabric_virtual_networks`
- `sda_host_port_onboarding`
- `site`
- `swim`
- `tags`
- `template`
- `user_role`
- `wired_campus_automation`
- `wireless_design`

### Config-generator roles

- `accesspoint_config_generator`
- `accesspoint_location_config_generator`
- `application_policy_config_generator`
- `assurance_device_health_score_settings_config_generator`
- `assurance_issue_config_generator`
- `backup_and_restore_config_generator`
- `device_credential_config_generator`
- `discovery_config_generator`
- `events_and_notifications_config_generator`
- `inventory_config_generator`
- `ise_radius_integration_config_generator`
- `network_profile_switching_config_generator`
- `network_profile_wireless_config_generator`
- `network_settings_config_generator`
- `pnp_config_generator`
- `provision_config_generator`
- `rma_config_generator`
- `sda_extranet_policies_config_generator`
- `sda_fabric_devices_config_generator`
- `sda_fabric_multicast_config_generator`
- `sda_fabric_sites_zones_config_generator`
- `sda_fabric_transits_config_generator`
- `sda_fabric_virtual_networks_config_generator`
- `sda_host_port_onboarding_config_generator`
- `site_config_generator`
- `tags_config_generator`
- `template_config_generator`
- `user_role_config_generator`
- `wired_campus_automation_config_generator`
- `wireless_design_config_generator`

## Naming Convention

- Workflow roles use the base module name directly. Example: `site` calls `cisco.catalystcenter.site_workflow_manager`.
- Config-generator roles append `_config_generator`. Example: `site_config_generator` calls `cisco.catalystcenter.site_playbook_config_generator`.

## Requirements

- Ansible >= 2.14
- `cisco.catalystcenter` collection
- Catalyst Center SDK >= 3.1.3.0.0
- Python >= 3.9

## Installation

```bash
ansible-galaxy collection install cisco.catalystcenter
```

## Usage

Each role now follows the conventional Ansible role scaffold used by `ansible-galaxy init`, including `tasks`, `defaults`, `handlers`, `vars`, `files`, `templates`, `tests`, `meta`, and `README.md`.

Example workflow role:

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

Example config-generator role:

```yaml
- hosts: localhost
  roles:
    - role: site_config_generator
      vars:
        catalystcenter_host: "{{ vault_catalystcenter_host }}"
        catalystcenter_username: "{{ vault_catalystcenter_username }}"
        catalystcenter_password: "{{ vault_catalystcenter_password }}"
        site_config_generator_file_path: "tmp/site_playbook_config.yml"
        site_config_generator_state: gathered
```

Each role also includes a local smoke harness under `tests/test.yml` with its companion `tests/inventory`.

## License

GPL-3.0-or-later

## Author

Cisco Systems
