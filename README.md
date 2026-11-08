# Extending Root Partition (sda1) on Linux easily (any cloud instance)

When extending the boot volume size in for example Oracle Cloud Infrastructure (OCI), your root disk (`/dev/sda`) expands, but intermediate boot/EFI partitions (`sda5`, `sda6`) may block direct in-place partition resizing. 

Using `growpart` (from `cloud-guest-utils`) safely handles the partition table adjustments without disturbing the boot configuration. It gives you far less of a hassle than using `parted` ...

---

## Step-by-Step Extension Guide

### 1. Install `cloud-guest-utils`
Install `growpart` to enable automatic partition table adjustments:

```bash
sudo apt update && sudo apt install -y cloud-guest-utils

```

> **Verification:**
> Run `which growpart` — it should return `/usr/bin/growpart`.

---

### 2. Expand Partition 1 Using `growpart`

Resize partition `1` (`sda1`) to occupy the newly allocated disk space:

```bash
sudo growpart /dev/sda 1

```

> **Verification:**
> Output should display a change confirmation, e.g.:
> `CHANGED: partition=1 start=... old: ... new: ...`

---

### 3. Resize the Filesystem

Expand the underlying `ext4` filesystem on `sda1` to consume the expanded partition:

```bash
sudo resize2fs /dev/sda1

```

> **Verification:**
> The command will output file system block growth details indicating success.

---

## Verification

Confirm that your root filesystem (`/`) reflects the increased storage capacity:

```bash
df -h
```
