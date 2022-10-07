## Home Assistant Add On for C-Bus to MQTT using cgate
Based on [cgateweb](https://github.com/the1laz/cgateweb), using [Clipsal CGate](https://updates.clipsal.com/ClipsalSoftwareDownload/mainsite/cis/technical/downloads/c-gate.html) and [ser2sock](https://github.com/nutechsoftware/ser2sock). 
Cgateweb source includes changes in PR #17 and #31, except I removed the console logging of NOOP because it results in excessive writing to the HA log file.
Thanks for help and support from @the1laz, @CraigD80, @frenck and others.
