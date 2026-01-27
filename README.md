# Juniper-SRX650-Fios-IPv6-PD
Working configuration on an SRX650 for a FiOS IPv6 PD deployment with IPv4 and
IPv6.

This repo contains two config snapshots:
- aklepptv.conf (current)
- v2 dhcpv6 added (older baseline with DHCPv6 changes)

Please leave notes on any errors or questions regarding configuration changes.
Not much exists on this older product with a PD deployment.
My DNS access pool is still not working as expected. I'm waiting on peer review
to confirm changes to resolve IPv6 DNS. With Google and OpenDNS, AAAA records
are offered.

For review notes and recommended updates, see [JMCP_REVIEW.md](JMCP_REVIEW.md).
