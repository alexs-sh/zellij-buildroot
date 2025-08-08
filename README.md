# About

[![Build Status](https://gitlab.com/alexs-sh/zellij-buildroot/badges/master/pipeline.svg)](https://gitlab.com/alexs-sh/zellij-buildroot/-/commits/master)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A helper project to integrate [zellij](https://github.com/zellij-org/zellij)
into [Buildroot](https://buildroot.org/). The patches submitted to the Buildroot
team can be found here [here](https://patchwork.ozlabs.org/project/buildroot/list/?series=&submitter=&state=&q=zellij&archive=&delegate=)

# Status

Zellij works on the test platforms: QEMU (AArch64), and Orange Pi (AArch64).
It seems it’s not easy to run it on 32-bit ARM due to limitations of
[cranelift-codegen](https://docs.rs/cranelift-codegen/0.122.0/cranelift_codegen/)
crate.

# Examples

**QEMU aarch64**

![QEMU](./pics/zellij-qemu.png "QEMU")

**Orange PI Zero**

![QEMU](./pics/zellij-orange.png "Orange PI")

# Build

The easiest way to build it is to use a script (like CI does).

```
git clone --recursive git@github.com:alexs-sh/zellij-buildroot.git
cd zellij-buildroot
./build-img.sh
```
Prepared images will be available at `buildroot/output/images` directory.

By default, the `build-img.sh` script builds a QEMU image. You can specify the
desired device by passing a configuration name to the script. For example,

```
./build-img.sh orangepi_zero2w_defconfig
```
Buildroot will generate images targeting the Orange PI.

Please visit the `configs` directory to view the full list of supported platforms.

You can avoid using the script, as it only performs trivial operations such as
placing the modified configuration file in the correct location. Instead, you
can manually replace the configuration file and run Buildroot directly. For
example:

```
cp configs/qemu_aarch64_virt_defconfig buildroot/configs/qemu_aarch64_virt_defconfig
cd buildroot
make qemu_aarch64_virt_defconfig
make
```

If you need to add new packages or change the configuration, you can use the
default Buildroot tools located in the `buildroot` directory. For example:

```
cd buildroot
make menuconfig
```

Please note, Buildroot has its own requirements regarding the applications that
should be installed on the host system. For more details, refer to the [System requirements](https://buildroot.org/downloads/manual/manual.html#requirement)
section in the Buildroot documentation. Alternatively, you can check the
`dockerfiles` directory which contains instructions to prepare Debian-based
images used for CI builds.
