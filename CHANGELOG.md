# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-10-17

### Added
- Support for `kernel.split_lock_mitigate = 0` to prevent CPU throttling/penalties on low-latency trading VMs (MetaTrader).
- Support for `zswap` in-kernel memory compression with `zstd` compressor to protect NVMe drives from raw swap I/O and reduce heat.

## [1.0.0] - 2026-10-14

### Added
- Initial release of `proxmox_resilience` role for Ansible Paradise.
- Support for kernel memory reserve tuning (`vm.min_free_kbytes`) to prevent memory starvation deadlocks.
- Support for kernel panic handling and automated recovery reboot (`kernel.panic = 10`).
- Support for hung task panic trigger (`kernel.hung_task_panic = 1`, `kernel.hung_task_timeout_secs = 120`).
- Support for safe OOM-killer behavior (`vm.panic_on_oom = 0`) to preserve host uptime.
- Support for NMI watchdog verification (`kernel.nmi_watchdog = 1`).
- Support for swappiness optimization (`vm.swappiness = 10`).
