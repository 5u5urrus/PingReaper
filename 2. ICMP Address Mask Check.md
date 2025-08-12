# ICMP Address Mask Check (`icmp_address_mask_check`)

## Opening Scenario

During an internal penetration test, a legacy printer responded to a single ICMP packet with its full subnet mask. With that one response, the testers instantly knew the network size, the range of valid IPs, and how to efficiently sweep for other targets — without touching a single port scan.

## What This Test Is

The ICMP Address Mask feature was introduced to help hosts discover their subnet mask on boot when no other configuration was available.

* **Type 17** — Address Mask Request
* **Type 18** — Address Mask Reply

The reply contains the subnet mask (e.g., `255.255.255.0`) in a 32-bit field.

## Why This Matters

* Discloses the internal network’s subnet mask, which aids in network mapping.
* Allows attackers to determine the potential size of a target network without active scanning.
* May reveal poor segmentation or flat network structures in sensitive environments.

## How It Works

A Type 17 packet is sent to the target.
If the target supports the feature and is configured to respond, it will return a Type 18 packet containing its network mask in standard IPv4 format.
This is usually unnecessary in modern networks but may still be enabled in older or misconfigured devices.

## Realistic Scenarios

1. **Reconnaissance** – A penetration tester queries an old network switch and learns it uses a `255.255.0.0` mask. This reveals the internal VLAN spans 65,000+ possible hosts, allowing the tester to plan a high-reward lateral movement campaign.

2. **Silent Mapping** – During a black-box assessment, an attacker identifies a forgotten router with ICMP Address Mask enabled. By retrieving its `/24` mask, they know the exact range to scan, avoiding detection by focusing only on active IP space.

## Defensive Recommendations

* Block ICMP Types 17 and 18 at network perimeters and untrusted segments.
* Disable Address Mask Request handling on all devices unless absolutely necessary.
* Use secure provisioning methods (e.g., DHCP, static config) to set subnet masks.

