# unraid-sas-spindown

```
Spin down SAS drives in Unraid. 
Unraid does a nice job of controlling HDD's energy consumption (and probably 
longevity) by spinning down drives. Up until now (v6.8.3), SAS drives in the
array would not spin down; the method used by Unraid for spindown affects ATA
drives only.

This is a temporary(...) stopgap until Limetech adds this into mainline Unraid.
Packaged as an Unraid plugin for convenience.

The plugin installs two scripts: One which is triggered via an rsyslog hook
whenever Unraid decides to spin down a drive; if the drive is SAS, it spins it
down (STANDBY, powermode 3).
The second is a wrapper for "smartctl", working around its deficiency of not
supporting the "-n standby" flag for non-ATA drives.

This software is provided with no warranty, either expressed or implied. 
It works for me, it may or may not work for you. 
USE AT YOUR OWN RISK.

@doron 2020-09-18

(with help from the great Unraid community, @SimonF, @Cilusse and others)
```
## Change Log:
```
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
