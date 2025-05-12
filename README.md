# Arcadyan PRV33AX225E-B-1B Firmware Dump

This repository contains a firmware dump (`deskt.bin`) for the Arcadyan PRV33AX225E-B-1B, a GPON Wifi 6 Modem. The dump includes two Squashfs file systems extracted from the nand memory, along with details about its structure and memory layout.

## File Systems

- **Filesystem 1**:
  - Location: `deskt.bin.extracted/C0100/images/filesystem@1/data`
  - Build Date: 08 February 2023
  - Type: Squashfs

- **Filesystem 2 (Primary)**:
  - Location: `deskt.bin.extracted/37C0100/images/filesystem@1/data`
  - Build Date: 09 October 2023
  - Type: Squashfs
  - Note: "Default" refers to the primary/active file system used by the device.

The file systems are extracted from `deskt.bin` using `binwalk`.

## Firmware Structure (binwalk Output)

The firmware image (`deskt.bin`) contains compressed data, device tree blobs (DTB), and UBI partitions. Key components include:

- LZMA-compressed data for kernel and file systems.
- Device tree blobs at offsets `0xC0100` and `0x37C0100`.
- UBI images for storage partitions.

Full `binwalk` output:

```bash
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
DECIMAL                            HEXADECIMAL                        DESCRIPTION
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
106444                             0x19FCC                            SHA256 hash constants, little endian
122423                             0x1DE37                            PKCS DER hash, SHA256
122506                             0x1DE8A                            PKCS DER hash, SHA512
123498                             0x1E26A                            PKCS DER hash, SHA256
123569                             0x1E2B1                            PKCS DER hash, SHA256
123640                             0x1E2F8                            PKCS DER hash, SHA256
131698                             0x20272                            PKCS DER hash, SHA256
131769                             0x202B9                            PKCS DER hash, SHA256
133762                             0x20A82                            PKCS DER hash, SHA256
133833                             0x20AC9                            PKCS DER hash, SHA256
135168                             0x21000                            LZMA compressed data, properties: 0x5D, dictionary size: 8388608 bytes, compressed size: 13983 bytes, uncompressed size: 32896 bytes
149504                             0x24800                            LZMA compressed data, properties: 0x5D, dictionary size: 8388608 bytes, compressed size: 100934 bytes, uncompressed size: 262056 bytes
524288                             0x80000                            gzip compressed data, operating system: Unix, timestamp: 2023-02-08 03:13:12, total size: 9724 bytes
786688                             0xC0100                            Device tree blob (DTB), version: 17, CPU ID: 0, total size: 44094323 bytes
58458368                           0x37C0100                          Device tree blob (DTB), version: 17, CPU ID: 0, total size: 44128159 bytes
117178368                          0x6FC0000                          UBI image, version: 1, image size: 82182144 bytes
199360512                          0xBE20000                          UBI image, version: 1, image size: 11010048 bytes
210370560                          0xC8A0000                          UBI image, version: 1, image size: 1179648 bytes
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

## MTD Partitions

The `/proc/mtd` output describes the flash memory layout of the device:

```bash
dev:    size   erasesize  name
mtd0: 00080000 00020000 "bootloader"    # 512 KB, contains the bootloader
mtd1: 00040000 00020000 "romfile"       # 256 KB, configuration data
mtd2: 002a0b61 00020000 "kernel"        # ~2.6 MB, Linux kernel
mtd3: 02770000 00020000 "rootfs"        # 39.5 MB, root file system
mtd4: 03700000 00020000 "tclinux"       # 55 MB, combined kernel+rootfs
mtd5: 002a1375 00020000 "kernel_slave"  # ~2.6 MB, secondary kernel
mtd6: 02780000 00020000 "rootfs_slave"  # 39.5 MB, secondary root file system
mtd7: 03700000 00020000 "tclinux_slave" # 55 MB, secondary combined kernel+rootfs
mtd8: 00100000 00020000 "config"        # 1 MB, device configuration
mtd9: 05a00000 00020000 "data"          # 90 MB, user data
mtd10: 00240000 00020000 "reservearea"  # 2.25 MB, reserved space
```
## Notes

- The firmware dump is provided for research and educational purposes only.
- **Missing Partitions**: This dump does not include the UBIFS `/default` and `/data` partitions, which contain personalized data specific to each modem (e.g., configuration settings, unique identifiers).
