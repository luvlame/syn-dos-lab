# SYN Flood DoS Attack — Penetration Testing Lab

A hands-on denial-of-service lab demonstrating how a SYN flood attack overwhelms a target system's resources by exploiting the TCP handshake process.

## Objective

Simulate a SYN flood DoS attack in a controlled, isolated environment and measure its impact on the target VM's CPU and memory usage. The goal was to understand how resource exhaustion attacks work and how to measure their effectiveness.

## Attack Concept

A SYN flood exploits the TCP three-way handshake:

1. Attacker sends a **SYN** packet to the target
2. Target responds with **SYN-ACK** and allocates resources for the half-open connection
3. Attacker never sends the final **ACK**
4. Target waits for the ACK until it times out, tying up resources
5. Repeating this thousands of times **exhausts the target's connection table**

**Fragmentation (`-f`)** adds extra processing overhead — the target must reassemble each fragmented packet before handling it.

## Lab Environment

| Role | System | IP Address |
|------|--------|------------|
| Attacker | Kali Linux (VM 1) | 192.168.56.103 |
| Target | Kali Linux (VM 2) | 192.168.56.102 |
| Network | Host-Only (isolated) | — |

**Tools used:** Kali Linux 2025.4, Oracle VirtualBox

> **⚠️ Both VMs were on a host-only adapter network to ensure no external systems were impacted.**

## Setup & Configuration

### Network Adapter Settings

Both VMs were configured with:

- **Attached to:** Host-Only Adapter
- **Adapter Type:** Intel PRO/1000 MT Desktop (82540EM)
- **Promiscuous Mode:** Deny

![Network adapter configuration](image001.png)

### Connectivity Verification

IP addresses were confirmed on both machines using `ip a`:

- **Target:** `192.168.56.102`
- **Attacker:** `192.168.56.103`

Connectivity was verified using `ping` from the attacker to the target. Zero packet loss confirmed the VMs were properly networked.

![Target IP](images/target-ip.png)
![Attacker IP](images/attacker-ip.png)
![Ping test](images/ping-test.png)

## The Attack

**Command used (from attacker VM):**
```bash
sudo ping -f 192.168.56.102
