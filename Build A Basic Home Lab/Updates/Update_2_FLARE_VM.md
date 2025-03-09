## Update #2: **FLARE-VM** installation.
[**FLARE-VM**](https://github.com/mandiant/flare-vm) is a collection of installation scripts for Windows that enables quick deployment and maintenance of a reverse engineering environment on a virtual machine (VM).

**Installation & Configuration**:
- Create a VM for Windows 10, install the OS and VirtIO drivers using this [guide](https://www.youtube.com/watch?v=-KTTmrA3DX8)
- Disable Windows Updates using this [guide](https://www.windowscentral.com/how-stop-updates-installing-automatically-windows-10)
- Disable Tamper Protection and Windows Defender via GPO using this [guide](https://superuser.com/questions/1757339/how-to-permanently-disable-windows-defender-real-time-protection-with-gpo/1757341#1757341)
- Install FLARE-VM via PowerShell using the following resources:
  - [Official Github page](https://github.com/mandiant/flare-vm)
  - [LetsDefend - Flare-VM Installation](https://www.youtube.com/watch?v=BiSdnusy2AQ)
  - [Grant Collins - Build a Malware Analysis Lab](https://www.youtube.com/watch?v=rmSIm3BKu3Y&t=2311s)
- Create a VM snapshot to preserve the clean state.

***Outcome***:
- The installed FLARE-VM provides a fully configured reverse engineering environment with essential analysis tools.
