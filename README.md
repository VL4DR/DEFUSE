# DEFUSE
*DEFUSE* is a technique that enables bypassing Secure Boot on some NVMe solid state drives (SSDs) based on Phison E12/E16/E18 controllers.

# Documentation
Exploitation assumes we have *volatile* code execution on the target controller. See [PhisonExecutor](https://github.com/VL4DR/PhisonExecutor) repository that achieves this on some of the latest firmwares. Given this capability:
* We first dump the BootROM binary (the presumably *immutable* code that gets executed when power is applied to the controller). This is one of the possible avenues to gaining persistence, as persistent data that the BootROM processes during its execution is an attack surface. See [Dumping BootROM](./dumping_bootrom.md) section for more information.
* We map the persistent data the BootROM processes to find primitives that can lead to actual persistence. See [Finding Primitives](./finding_primitives.md) section for more information.
* We create a sequence of operations to exploit the primitives we found in the previous step to enable uploading unsigned firmware binary to the controller, persistently. As a prerequisite to understanding exploitation, please refer to [Firmware Update Process](./firmware_update.md) section. The exploitation steps can found under [Exploitation](./exploitation.md) section.

# Affected Devices
Uploading unsigned firmware applications was successful on the following drives:
* PNY CS3030 (Phison E12)
* Kingston KC3000 (Phison E18)
* Sabrent Rocket 4 Plus (Phison E18)
\
For transparency, the KC3000 drive's firmware appeared to not be signed, so this bypass might not be truly needed for that specific drive. However, the Rocket 4 Plus drive, at least based on the configured fuses, appeared to be signed properly, and this technique was demonstrated there successfully.\
Drives based on E12/E16/E18 controllers from other vendors (and other models) are probably also affected.

# Notes
* Due to the nature of the vulnerability, no exploitation code is provided here. Also, since the BootROM in this case is presumably immutable, this issue *cannot* be patched for already-shipped devices.
* The attack vector is local and requires root/administrator privileges on the host communicating with the controller, since exploitation involves sending Vendor Specific Commands (VSCs). However, physical access to the drive is *not* required.
* The documentation is based on our understanding of firmware code, so it is not guaranteed to be perfect. If some information is inaccurate or some clarifications are needed, you can email log1kxd at gmail.com.
