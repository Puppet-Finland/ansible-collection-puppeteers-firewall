# Ansible Collection - puppeteers.firewall

Ansible roles to do opinionated firewalld configuration on RHEL 9 and
derivatives.

# Roles

## base

Configure very basic firewall settings for the "public" zone. Parameters:

* *firewall_base_manage_firewall:* whether to actually manage firewalld rules or not
* *firewall_base_public_interface:* the identifier for the public interface
