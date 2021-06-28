# UD-Mobile-iOS
Issue-only repository for tracking issues related to the UD Mobile iOS App

# iOS TestFlight 
Join iOS TestFlight by following instructions here https://testflight.apple.com/join/xHtzI5R3.  Note that iOS TestFlight versions are only valid for 90 days. Please keep a UD Mobile backup in the event a build expires.


# Reporting an Issue
Please open an [issue](https://github.com/UniversalDevicesInc/UD-Mobile-iOS/issues).
Please check if the issue already exists to avoid duplicates. If issue exists, please leave a comment in the existing issue.
Include UD Mobile version number and ISY Firmware version in report.

# Installation
Apple App Store https://apps.apple.com/us/app/ud-mobile/id1550618148?itsct=apps_box_badge&itscg=30200

# Remote Connections
Remote connection on UD Mobile can be achieved in 3 scenarios.

First is our managed method using ISY Portal. ISY Portal has competitive prices of $23 for the first 2 years and renewals cost of $20 for two years (prices current as of 06/2021). If your ISY has not been associated with the ISY Portal in the past we offer a 30 day free trial.

Second is an unmanaged direct connection. The unmanaged method requires a Trusted CA Signed SSL Certificate. The Trusted SSL CERT is required for reasons stated below in App Transport Security. Instructions on adding a CERT to your ISY can be found here: https://www.universal-devices.com/docs/production/ISY994%20Series%20Network%20Security%20Guide.pdf . There are methods to add a Self Signed CERT to the Trusted Key Store on Android, however it is beyond the scope of our support and may require root on some devices.

Finally a local connection can be established on a remote network if running a VPN Server on the same local network as the ISY. To use this method select "Only use Local Connection" in the local connection settings. Setting this option will instruct the App to ignore remote connection settings and only use the local network.

### App Transport Security (ATS)
App Transport Security (ATS) is disabled by iOS for local loads (1), for this reason Local Connections do not need a Trusted CERT, ATS requires a Trusted CERT for Remote Connections.
While it is possible for the apps to disable ATS it would make all connections less secure and we would have to meet the exception requirements (2) during app review which we likely do not meet.

(1) https://developer.apple.com/documentation/bundleresources/information_property_list/nsapptransportsecurity/nsallowslocalnetworking " ATS doesn’t block local loads by default in newer versions of the OS"

(2) https://developer.apple.com/documentation/security/preventing_insecure_network_connections#3138036 

"The app must connect to a server managed by another entity that doesn’t support secure connections."  Portals provided by UDI and Third parties support secure connections, and UDI controls the firmware. So, this does not apply.  Apple has also mentioned this will be removed in the future.

"The app must support connecting to devices that cannot be upgraded to use secure connections, and that must be accessed using public host names.".  ISY firmware does support secure connections, so this exception does not apply.


# Node Servers
Please reboot the ISY if Node Server values are not formatted or are formatted incorrectly.  This is needed for all new Node Server installs and may be needed if NodeDef, Editor, or NLS files for a Node Server have been changed during an update.  After rebooting the ISY UD Mobile may need to sync with the ISY to obtain the new files. Incorrect formatting only affects clients, including UD Mobile, which read the formatted value from the ISY subscription.  Incorrect formatting does not appear to affect the Admin Console.

# Multiple Systems
UD Mobile supports many ISYs simultaneously without the need to switch profiles and they can all be combined in favorites.  There may be a limit on the number of simultaneous subscriptions based on the user's device (multi-threading), however we have not had reports of this limit being reached.  The app also allows the user enable/disable subscriptions by going to Settings-Tab > Systems > ISY-Name > Advanced > Enable.  Note that the more systems enabled the longer it will take to populating initial status values, same with enabling Program/Variable values at startup.  The average ISY had about 300 - 1200 status values with a status insertion completion time from open at about 2-7 seconds (local) and 5-12 seconds (remote) with a modern mobile processer. If we multiply status value insert time by number of ISY's it can add up quickly.

To add a new ISY go to Settings-Tab > Systems and then click the "Add" button on the top right.  If all ISY's are on the same portal account, it can be reused after linked the first time.  The app will also support multiple portal accounts if needed. 

# Multiple ISY Portal Accounts
UD Mobile can support may ISY Portal Accounts although it is recommended users use the same ISY Portal Account for all their ISYs.  This feature is intended for installers and developers.  To add a new portal account, go to Settings-Tab > Portals > and then click "Add" button on the top right.  A new portal account can also be added when adding a new System by clicking yes when prompted to use ISY Portal, then clicking "Add New".

# Missing Data
We have reports of UD Mobile missing data and controls which is caused by incomplete firmware installation.  To verify this is your issue make the following request in a browser (preferably Chrome) on the same local network as the ISY. Replace ipAddress with the local IP Address of the ISY. Be sure to use Insteon request for Insteon Firmware and Zwave Request for Zwave only firmware.

Insteon Request for missing Status and Controls:
http://ipAddress/rest/profiles/family/1/files  

Zwave request for missing Status and Controls:
http://ipAddress/rest/profiles/family/4/files

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

If file names are empty or missing firmware will need to re-installed or upgraded.  Reinstallation must be done from the Admin Console when on the same local network as the ISY.
```xml 
<file name="I_EDIT.XML"/>
```      


