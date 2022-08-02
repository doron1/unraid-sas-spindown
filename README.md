# unraid-sas-spindown

```
Spin down SAS drives in Unraid. 
Unraid does a nice job of controlling HDD's energy consumption (and probably 
longevity) by spinning down drives. That works very well for SATA drives; 
however SAS drives are quite peculiar in their power mode controls. Specifically
they do not respond to ATA standby (spin down) commands.

This plugin add SAS drive spin down support to Unraid. 

The plugin is installed in two very differnt ways, depending on the Unraid OS
version. For versions up until v6.8.3, the plugin installs two components: 
One which is triggered via an rsyslog hook whenever Unraid decides to spin down 
a drive, and does "the right thing" for SAS. The second is a wrapper for "smartctl", 
working around its deficiency of not supporting the "-n standby" flag for non-ATA drives.
From version 6.9.0, Unraid OS already includes the wrapper, and implemented a modular
script for spin up/down control. For those versions, the plugin enhances the functionality
via that mechanism.

Some SAS drives and controllers do not respond favorably to the SAS STANDBY command
(POWERMODE 3). Towards that end, the plugin also implements an exclusion filter, to
try and avoid all sorts of red x situations.

This software is provided with no warranty, either expressed or implied. 
It works for me, it may or may not work for you. 
USE AT YOUR OWN RISK.

@doron 2020-09-18

(with help from the great Unraid community, @SimonF, @Cilusse and others)
```
## Change Log:
```
2022.08.02            Align return codes from "sdspin" (also per 6.11 changes)

2022.05.25            Issue some more debug messages to syslog (vs. interactive "echo") 
                      when debug is set

2022.04.25            Change version numbering to match common plugin wisdom.
                      Minor code tidy-ups.

2021-08-17 v0.86      With Unraid 6.10, *some* SAS drives were spinning back up immediately;
                      probably due to changes in the SAS controller driver.
                      SAS drive detection logic revamped to work around the bug.
                      Also streamlined the exit code logic in sdspin.

2021-02-06 v0.85      In some configurations, the latest version of "exclusions" file was not
                      being pulled in. Thanks @xaositek.

2020-12-16 v0.82      In some configurations, SATA drives were mis-classified as SAS drives

2020-12-11 v0.8       Update for Unraid 6.9.0, use new spin up/down hook. Plugin now supports 
                      both old and new systems.
                      When spinning down off of rsyslog, send the sg_start command to the background 
                      so as not to block rsyslog if things get funny.
                      Use sdparm rather than smartctl to detect if a drive is SAS.
                      If smartctl version >= 7.2 or Unraid >= 6.9.0-rc1, do not install wrapper.
                      Other fixes and imrpovements...

2020-10-07 v0.7       Filter out syslog lines from some SAS devices rejecting ATA standby op (e0) issued by mdcmd
                      Consistent log messages' tag
                      Add some debug and testing tools
                      Implement an exclusion list. Initially for some SEAGATE and HITACHI SAS drives
                      When smartctl is evaded, give exit code 2 - as smartctl does with ATA. Thanks @segator
                      Adapt syslog include files to various Unraid configs :-(
                      Reconfigure filter each time rsyslog.conf is modified.
                      Many other fixes and improvements, major code reorg
   Thanks @SimonF for great feedback and suggesstions, thanks @jowe and others for testing efforts.

2020-10-01 v0.6       Update support forum thread (new one, under Plugin Support)
                      Limit action to rotational drives (probably already covered by Unraid but making sure)
                      More code tidy-up
2020-09-26 V0.5       Pack as a plugin
2020-09-22 V0.4       Some more cosmetic tidy-ups
2020-09-20 V0.3       Typo in message
2020-09-20 V0.2       Fix a stupid packing bug
                      Some code tidy-up                     
                      Include version in messages
                      Log the installation
                      
```
