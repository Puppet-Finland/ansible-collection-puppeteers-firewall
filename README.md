# Ansible Collection - puppeteers.firewall

Ansible roles to do opinionated firewalld configuration on RHEL 9 and
derivatives.

# Roles

## base

Configure very basic firewall settings for the "public" zone. Parameters:

* *firewall_base_manage_firewall:* whether to actually manage firewalld rules or not
* *firewall_base_public_interface:* the identifier for the public interface

Existing firewall rules are not purged.

## strict

This role sets up a very strict firewalld rules that split traffic into zones based on the source IPs:

* VPN traffic (zone name: 50_vpn, SSH service by default)
* Backup traffic (zone name: 10_backup, SSH service by default)
* Monitoring traffic (zone name: 11_monitoring, port 9100, i.e. Node Exporter) by default

The rules are prefixed with numbers for a reason: firewalld loads zone
configuration in alphabetical order and uses the rules that match first. In
other words more specific rules (e.g. single-IP zones) need to be defined
before more generic rules (e.g. match a network). For details see [this
article](https://access.redhat.com/solutions/5663141).

Additionally the role can attach a list of interfaces to the "drop" zone,
effectively disabling all traffic that is not explicitly authorized above:

    puppeteers_firewall_strict_assign_to_drop_zone:
      - eth0
      - eth1

You can disable certain zones by setting their respective manage parameters to false:

* puppeteers_firewall_strict_manage_vpn_zone
* puppeteers_firewall_strict_manage_backup_zone
* puppeteers_firewall_strict_manage_monitoring_zone

At minimum, you need to define the source IPs for each zone you wish to manage:

    puppeteers_firewall_strict_backup_source: '10.15.0.10/32'
    puppeteers_firewall_strict_monitoring_source: '10.15.0.20/32'
    puppeteers_firewall_strict_vpn_source: '10.174.12.0/24'

These IPs / IP ranges may not overlap.

You can define which services and/or ports to allow for each zone. See [roles/strict/defaults/main.yml](roles/strict/defaults/main.yml) for details.
