# CiscoMacTraces
EXE for Hardware MAC adress tracing on Cisco switches 
Cisco MAC Trace (Walyu) – README
Version: 0.1.6

Overview
--------
Cisco MAC Trace is a small Windows GUI tool that helps you locate where a given MAC address is learned in a Cisco IOS/IOS-XE switching environment.
It connects via SSH to one or more “starting” switches, follows CDP-trunk uplinks hop-by-hop, and stops when it reaches a non-trunk (or non-CDP-matched) port.
It then prints:
- the switch IP + hostname
- the final port
- the MAC table entries on that port
- the LLDP neighbors line for that port

Files
-----
Typical portable folder layout (recommended):
- CiscoMacTrace0.1.6.exe
- mactrace.cfg              (optional, for default values and UI labels)

Note:
If mactrace.cfg is missing, the application will NOT pre-fill any default IPs or username.

Configuration (mactrace.cfg)
----------------------------
This tool reads a simple key=value config file named: mactrace.cfg
Place it next to the EXE.

Example:
CiscoIPs=10.0.110.87,10.0.110.63
username=username

UI labels (optional):
FieldIP="Switch IPs (comma-separated):"
FieldMAC="MAC address (xxxx.xxxx.xxxx):"
FieldUSER="Username:"
FieldPW="Password:"
FieldPRIV="Enable password (optional):"

Menu labels / button labels (optional):
MenuFile="File"
MenuSave="Save result"
MenuCopy="Copy result"
MenuExit="Exit"
MenuAbout="About"
ButtonCopy="Copy result"


Usage
-----
1) Launch CiscoMacTrace0.1.6.exe
2) Enter:
   - Switch IPs (comma-separated): e.g. 10.0.110.87,10.0.110.63
   - MAC address: accepts many formats (upper/lower, with/without separators). It will be normalized to Cisco format (xxxx.xxxx.xxxx).
   - Username / Password (SSH credentials)
   - Enable password (optional) if your devices require enable
3) Click “Trace”

The result is printed in the main text area.
You can also:
- File -> Save result (default filename: MAC without dots + .txt)
- File -> Copy result (clipboard)
- File -> Exit
- File -> About

MAC input validation / normalization
-----------------------------------
- The tool removes all non-hex characters, lowercases the result, then validates that the final string is exactly 12 hex characters (0-9, a-f).
- If valid, it is reformatted as xxxx.xxxx.xxxx before any network lookup.
- If invalid (wrong length or non-hex characters), the trace is blocked with an error message.

Troubleshooting
---------------
1) EXE “does nothing”
   Build once without --noconsole to see Python runtime errors:
   py -m PyInstaller --onefile --name CiscoMacTrace0.1.6 --icon mactrace.ico --version-file version.txt --add-data "walyu_logo.png;." mac_trace_gui_v0.1.6.py

2) Antivirus / SmartScreen warnings
   Unsigned freshly-built EXEs can be flagged. Consider code signing (Authenticode) for broad deployment.

3) SSH connection failures
   - Verify TCP/22 reachability from the PC to the switch
   - Check credentials
   - Confirm SSH is enabled on the switch
   - Ensure IOS/IOS-XE device_type is appropriate (this tool targets Cisco IOS/IOS-XE)

4) CDP / LLDP dependencies
   - Hop-to-hop switching relies on CDP neighbors + trunk detection.
   - Final neighbor info is based on “show lldp neighbors” output (summary filtered to the target port).

License / Credits
-----------------
This software has been developed by Walyu PTE LTD – IT research department.
https://www.walyu.com
Incorporated in Singapore.
(C) Copywright 2026
