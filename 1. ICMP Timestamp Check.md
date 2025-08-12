# ICMP Timestamp Check (`icmp_timestamp_check`)

## Opening Scenario

During a security assessment, a simple ping-like packet gave away the target’s exact system time down to the millisecond. With that single piece of information, the testers could align perfectly with the server’s internal clock — opening the door to time-based attacks and valuable reconnaissance.

## What This Test Is

The ICMP Timestamp feature is part of the original ICMP specification:

* **Type 13** — Timestamp Request
* **Type 14** — Timestamp Reply

The reply contains three timestamps (milliseconds since midnight UTC):

* **Originate Timestamp** – When the request left the sender.
* **Receive Timestamp** – When the request reached the target.
* **Transmit Timestamp** – When the target sent the reply.

## Why This Matters

* Reveals precise system time, enabling time-based attack synchronization.
* Allows clock skew analysis for host fingerprinting and tracking.
* Can disclose differences in time sources across a network, hinting at topology or virtualization.

## How It Works

A Timestamp Request (Type 13) is sent to the target.
A compliant system replies with a Timestamp Reply (Type 14) containing its current time values.
By comparing these against the tester’s local clock, it’s possible to calculate drift and identify unique timekeeping patterns.

## Realistic Scenarios

1. **Direct Exploitation** – An internal app server responds with timestamps showing its clock is two minutes ahead of the corporate NTP source. The red team uses this offset to precompute valid time-based session tokens before they become active, bypassing access controls.

2. **Intelligence Gathering** – Multiple probes over several minutes reveal a consistent clock skew signature matching VMware ESXi’s known drift pattern. Without touching banners or services, the attacker deduces the host is a VM, narrowing exploit choices to known ESXi vulnerabilities.

## Defensive Recommendations

* Block ICMP Types 13 and 14 at the network perimeter.
* Use authenticated NTP for time synchronization.
* Restrict timestamp responses to trusted internal hosts only.
