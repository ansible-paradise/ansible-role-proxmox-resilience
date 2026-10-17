# ansible-role-proxmox-resilience

[![CI](https://github.com/ansible-paradise/ansible-role-proxmox-resilience/actions/workflows/ci.yml/badge.svg)](https://github.com/ansible-paradise/ansible-role-proxmox-resilience/actions)
[![Maintained by HeidelSec](https://img.shields.io/badge/Maintained%20by-HeidelSec-0052cc?logo=shield)](https://github.com/heidelsec)

Ansible role to harden Proxmox VE hosts and Linux hypervisors against hard freezes, memory starvation lockups, and unrecoverable kernel panics via low-level sysctl and kernel tuning.

Designed specifically for homelab hypervisors running mixed workloads (high-frequency trading VMs, large containers, and RAM-intensive stacks), preventing silent host freezes when guest memory spikes.

## Requirements

- Target: Proxmox VE 7 / 8 / 9 (Debian 11 / 12 / 13)
- Ansible >= 2.15
- Collection: `ansible.posix` >= 1.5.0

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `proxmox_resilience_enabled` | `true` | Master toggle for the role |
| `proxmox_resilience_sysctl_file` | `/etc/sysctl.d/99-homelab-resilience.conf` | Path to persistent sysctl config file |
| `proxmox_resilience_vm_min_free_kbytes` | `1048576` | Minimum kernel reserve memory in KB (1 GB headroom prevents I/O deadlocks) |
| `proxmox_resilience_vm_swappiness` | `10` | Swappiness ratio (preserves memory page cache) |
| `proxmox_resilience_vm_panic_on_oom` | `0` | Disables host panic on OOM; forces kernel OOM killer to terminate rogue processes |
| `proxmox_resilience_kernel_panic` | `10` | Seconds before automated reboot after kernel panic |
| `proxmox_resilience_kernel_hung_task_panic` | `1` | Forces panic and automated reboot on stuck tasks |
| `proxmox_resilience_kernel_hung_task_timeout_secs` | `120` | Threshold in seconds before a stuck task triggers panic |
| `proxmox_resilience_kernel_nmi_watchdog` | `1` | Ensures hardware NMI watchdog detects hard CPU core lockups |
| `proxmox_resilience_kernel_split_lock_mitigate` | `0` | Disables artificial CPU sleep penalty on x86 split-lock traps (crucial for MetaTrader VMs) |
| `proxmox_resilience_zswap_enabled` | `true` | Enables in-memory compressed swap buffer to protect NVMe drives and prevent I/O freezes |
| `proxmox_resilience_zswap_compressor` | `zstd` | Compression algorithm for zswap pool (`zstd` gives best speed/ratio) |
| `proxmox_resilience_zswap_max_pool_percent` | `20` | Max percentage of host RAM allocated to compressed zswap pool |

## Example Playbook

```yaml
---
- name: Harden Proxmox Hypervisor Hosts
  hosts: proxmox_nodes
  roles:
    - role: ansible-paradise.proxmox_resilience
```

Or install via Git in `requirements.yml`:

```yaml
---
roles:
  - name: ansible-paradise.proxmox_resilience
    src: git+https://github.com/ansible-paradise/ansible-role-proxmox-resilience.git
    version: main
```

## License

[MIT](LICENSE)

## Author & Maintenance

Maintained by [ansible-paradise](https://github.com/ansible-paradise), an open-source initiative by [HeidelSec](https://heidelsec.de). Enterprise infrastructure hardening and security: [heidelsec.de](https://heidelsec.de).
