# linux-nvme

This github repository contains: <br /> 
* NVME kernel patches for linux made by Andy Lutomirski. <br /> 
* PKGBUILD for linux-nvme on Arch maintained by me. <br /> 

These patches enable NVME drives to enter lower power states.<br />
For example: my case (XPS13, linux-nvme4.9.0) it decreases idle usage by ~1.5watt (see Benchmarks file)<br />
<br />
## 5 Ways to install:
#### 1) ARCH manually compile kernels: (EASY/SLOW)

* git clone: https://github.com/damige/linux-nvme.git
* go into /src/[kernel you want]
* type "makepkg" -wait until compilation completes-
* pacman -U linux-nvme-*
* Adjust your bootloader to boot linux-nvme
<br />

#### 2) non-ARCH/ARCH Patch kernel of choice: (HARD/SLOW)
4.8.x:<br />
Patch using nvmepatch1-V4.patch, nvmepatch2-V4.patch, nvmepatch3-V4.patch.
<br />
4.9.x:<br />
Patch using APST.patch, pm_qos1.patch, pm_qos2.patch, pm_qos3.patch, nvme.patch
<br />
4.10.x:<br />
Patch using APST.patch
<br />
#### 3) ARCH install from AUR: linux-nvme (EASY/SLOW)
<br />
Adjust your bootloader to boot linux-nvme
<br />
### If you chose to trust me compiling for you:<br />
#### 4) ARCH REPO: Add this to your /etc/pacman.conf (EASY/FAST)<br />
```
[linuxnvme]
SigLevel = Never
Server = http://linuxnvme.damige.net/repo
```
<br />
install with:
```
pacman -S linuxnvme/linux-nvme
```
<br />
Adjust your bootloader to boot linux-nvme
<br />
#### 5) ARCH binary download: (EASY/FAST)
http://linuxnvme.damige.net/
<br />
install with:
```
pacman -U linux-nvme-*
```
<br />
Adjust your bootloader to boot linux-nvme
<br />
<br />
### To test if the APST is working try:
<br />
* install nvme-cli and: "nvme get-feature -f 0x0c -H /dev/nvme0" Expected output is: "Autonomous Power State Transition Enable (APSTE): Enabled"
<br />
* Check if "/sys/class/nvme/nvme0/power/pm_qos_latency_tolerance_us" exists 
<br />
* Verify measurably lower power usage when ssd is idle
<br />

### Known issues:
In the latest patches the samsung SM951 (as used in the XPS 9350) has been disabled for NVME APST.
This is due to some reported instability.<br />
I have not included the patch disabling this nvme drive, as i use it myself. Incase you want the full APST patch its listed as APST-FULL.patch<br />
I have had a single lock i can attribute to the ssd in the last 60 days, for me the benifit of these patches outways the occasional issue with specifically the SM951.
