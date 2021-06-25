# UD-Mobile-iOS
Issue-only repository for tracking issues related to the UD Mobile iOS App

# TEST FLIGHT USERS
App only supports nodes (Insteon, Zwave, and Node Servers), Programs, Variables, Scenes, and Network Resources. Favorites have not been added. At this time we are looking for ANR (app not respondin) and crashes.  Please hold feature requests until further notice.


## Reporting an Issue
Please open an [issue](https://github.com/UniversalDevicesInc/UD-Mobile-iOS/issues).
Please check if the issue already exists to avoid duplicates. If issue exists please leave a comment in the existing issue.
Include UD Mobile version number and ISY Firmware version in report.

## Installation
Test Flight ONLY, by invitation only.

## Remote Connections
If not using the ISY Portal, UD Mobile supports remote connections by A) VPN and selecting "Only Use Local Connection" or B) A valid CERT. This will not work with self signed CERTs unless trusted by the operating system. Adding a self signed CERT to an OS is beyond the scope of support for this app.
Apple and Google have given developers notice that bypassing security may result in removal from the app stores. While it is currently possible to bypass the https CERT there is no guarantee it will work on the next version or that it will not result in removal from the respective app platforms.  

# Node Servers
Please reboot the ISY if Node Server values are not formatted or are formatted incorrectly.  This is needed for all new Node Server installs and may be needed if NodeDef, Editor, or NLS files for a Node Server have been changed during an update.  After rebooting the ISY UD Mobile may need to sync with the ISY to obtain the new files. Incorrect formatting only affects clients, including UD Mobile, which read the formatted value from the ISY subscription.  Incorrect formatting does not appear to affect the Admin Conslole.

# Missing Data
We have reports of UD Mobile missing data and controls which is caused by incomplete firmware installation.  To verify this is your issue make the following request in a browser (preferably Chrome) on the same local network as the ISY. Replace ipAddress with the local IP Address of the ISY. Be sure to use Insteon request for Insteon Firmware and Zwave Request for Zwave only firmware.

Insteon Request for missing Status and Controls:
http://ipAddress/rest/profiles/family/1/files  

Zwave request for missing Status and Controls:
http://192.168.1.30/rest/profiles/family/4/files

Insteon should return the following.  
      
```xml
<profiles>
<profile family="1" id="1">
<files dir="EDITOR">
<file name="I_EDIT.XML"/>
</files>
<files dir="EMAP">
<file name="I_EMAP.XML"/>
</files>
<files dir="LINKDEF">
<file name="I_LDEFS.XML"/>
</files>
<files dir="NLS">
<file name="EN_US.TXT"/>
</files>
<files dir="NODEDEF">
<file name="I_NDEFS.XML"/>
</files>
</profile>
</profiles>
```

Zwave should return the following:

```xml
<profiles>
<profile family="4" id="1">
<files dir="EDITOR">
<file name="EDITORS.XML"/>
</files>
<files dir="LINKDEF">
<file name="ZW_LDEFS.XML"/>
</files>
<files dir="NLS">
<file name="EN_US.TXT"/>
</files>
<files dir="NODEDEF"/>
<files dir="EMAP"/>
</profile>
</profiles>
```

If family names are empty or missing firmware will need to reinstalled.  Reinstallation must be done from the Admin Console when on the same local network as the ISY.
```xml 
<file name="I_EDIT.XML"/>
```      
