# oci-wireguard-vpn
# Self-Hosted WireGuard VPN on Oracle Cloud

I built a personal WireGuard VPN on an Ubuntu virtual machine in Oracle Cloud Infrastructure (OCI). The VPN routes client IPv4 traffic through the cloud server. I documented the setup from the OCI network configuration through WireGuard client testing so others can reproduce it with their own accounts and devices.

## Setup Guide

[Download the step-by-step setup guide](./OCI_WireGuard_VPN_Setup_Guide.docx)

The guide includes the commands, configuration templates, and verification steps for:

- **OCI networking** — Create a virtual cloud network and public subnet, assign a reserved public IPv4 address, and configure ingress rules for SSH and WireGuard.
- **Ubuntu hardening** — Create a dedicated admin account, configure SSH key authentication, and set up UFW, Fail2ban, and automatic security updates.
- **WireGuard** — Configure an IPv4 full tunnel with IP forwarding and NAT. Give each device its own key pair and VPN address.
- **Client setup and testing** — Import the phone configuration with a QR code, add more devices, verify the WireGuard handshake and outbound public IP, and test SSH over the VPN before removing public SSH access.

## What I Learned

While setting up the VPN, I ran into a handshake failure even though UFW allowed UDP port `51820`. A packet capture showed the requests reaching the VM. When I checked the underlying `iptables` rules, I found that a reject rule was evaluated before the UFW rules. Placing the WireGuard allow rule ahead of that reject resolved the handshake issue.

That troubleshooting step taught me to check both cloud and host firewall layers and look at the order in which rules are applied. I included the diagnostic commands and the fix in the guide.
