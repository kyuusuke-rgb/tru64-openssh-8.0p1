# OpenSSH 8.0p1 for Tru64 UNIX (Alpha Architecture)

This repository provides pre-compiled OpenSSH 8.0p1 binaries, essential shared libraries, and installation scripts for Tru64 UNIX V5.1B running on DEC Alpha systems.

## Prerequisites

- **OS**: Tru64 UNIX V5.1B (Alpha)
- **Privilege**: Root access (`#`)

## Pre-compiled Release Assets

You can download the compiled archives from the [Releases](../../releases) section:

- `openssh8-openssl111-tru64-v51b-portable.tar.gz`: Portable OpenSSH 8.0p1 binaries, OpenSSL 1.1.1 libraries, and configuration files.
- `tru64_essential_libs.tar.gz`: Minimal set of dependency shared libraries required to run GCC and OpenSSH (e.g., `libmpc`, `libdb-3.1`, `libgcc_s`, `libgmp`, `libmpfr`, `libz`).

---

## Quick Start & Installation

### 1. Extract Essential Shared Libraries

Extract the required shared libraries to `/usr/local/lib`:

```sh
# mkdir -p /usr/local/lib
# cd /usr/local/lib
# tar -zxvf /path/to/tru64_essential_libs.tar.gz
