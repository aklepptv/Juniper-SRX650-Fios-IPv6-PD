# JMCP Review: Repository Update Recommendations

Scope: SRX650 FiOS IPv6 PD configuration snapshots and README guidance.

## High-priority checks
1) Validate the IPv6 delegated prefix pool format in `aklepptv.conf`:
   - `access address-assignment pool v6-pd-pool family inet6 prefix 2600:4041:::/64;`
   - The `:::/64` syntax is invalid; replace with a valid prefix and ensure it
     matches the ISP delegated /56 (or update the pool to reflect your actual
     delegation size).
2) Router advertisements are enabled on the WAN interface:
   - `protocols router-advertisement interface ge-0/0/1.0;`
   - Typically RAs should be on LAN interfaces only. Confirm you are not
     advertising a LAN prefix on the untrust side.
3) Ensure the LAN interface participates in IPv6:
   - `interfaces xe-2/0/0 unit 0` lacks `family inet6` in `aklepptv.conf`.
   - Without `family inet6`, LAN neighbor discovery and RA delivery can fail.

## Medium-priority checks
- `client-type statefull` appears in both configs. Junos expects `stateful`;
  confirm the spelling matches the syntax for your Junos release.
- The WAN interface uses IPv4 DHCP while a static default route is configured.
  Verify the static route is required and does not conflict with DHCP options.
- Trust zone inbound services are set to `all`. Consider restricting inbound
  services and protocols to the minimum required set.

## DNS and RA behavior
- The README notes DNS via DHCPv6 is inconsistent. In `v2 dhcpv6 added`, the RA
  on the LAN interface is marked `managed-configuration` and
  `other-stateful-configuration`. In `aklepptv.conf`, those flags are missing.
  If clients are not requesting DHCPv6 DNS options, consider enabling these RA
  flags or using RDNSS if supported on your Junos release.

## Repo hygiene
- Consider renaming `v2 dhcpv6 added` to `v2-dhcpv6-added.conf` for tooling and
  documentation clarity.
- Add a short "apply safely" snippet (for example: `commit check` on a lab box)
  to the README to reduce the risk of unintended changes.
