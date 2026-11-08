# DEFUSE
*DEFUSE* is a technique that enables bypassing Secure Boot on some NVMe Phison solid state drives (SSDs) based on E12/E16/E18 controllers.

# Documentation
Exploitation assumes we have *volatile* code execution on the target controller. See PhisonExecutor repository that achieves this on some of the latest firmwares. Given this capability:
* We first dump the BootROM binary (the presumably *immutable* code that gets executed when power is applied to the controller). This is one of the possible avenues to gaining persistence, as persistent data that the BootROM processes during its execution is an attack surface. See [Dumping BootROM](./dumping_bootrom.md) section for more information.
* We map the persistent data the BootROM processes to find primitives that can lead to actual persistence. See [Finding Primitives](./finding_primitives.md) section for more information.
* We create a sequence of operations to exploit the primitives we found in the previous step to enable uploading unsigned firmware binary to the controller, persistently. As a prerequisite to understanding exploitation, please refer to [Firmware Update Process](./firmware_update.md) section. The exploitation steps can found under [Exploitation](./exploitation.md) section.

# Notes
* Due to the nature of the vulnerability, no exploitation code is provided here.
* The attack vector is local and requires root/administrator privileges on the host communicating with the controller, since exploitation involves sending Vendor Specific Commands (VSCs).
* The documentation is based on our understanding of firmware code, so it is not guaranteed to be perfect. If some information is inaccurate or some clarifications are needed, you can email log1kxd at gmail.com.
