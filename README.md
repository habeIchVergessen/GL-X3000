# GL-X3000

* monitor gpsd, gpxlogger
* serve gpx-track's on website
* report GPS-position via sms (poll incoming messages and reply to accepted masters)
* upload gpx-track's to remote website

## Configuration - First use

* download GL-X3000 gpx.tgz

### extract to sd card
* cd /tmp/mountd/disk1_part1
* tar xvfz <name>.tgz
* cd
* ln -s /tmp/mountd/disk1_part1/scripts scripts
### check config
* ./scripts/config.sh
#### install missing packages
* opkg install [list of packages from output]
#### copy/link files listed above
* ln -s /tmp/mountd/disk1_part1/.config/etc/nginx/gl-conf.d/service-gps.conf /etc/nginx/gl-conf.d/service-gps.conf
#### check uci settings (/tmp/mountd/disk1_part1/.uci)
* uci show gpsd
* compare output off command with file gpsd.uci (gpsd.core.parameters needs to be added)
* restart gpsd
## first run
* ./scripts/gpxlogger-cron.sh
* logread -e gpx && date
* output like this should shown: 05.07.2023 11:29.00 CEST: starting gpxlogger
### additional tests
* ./scripts/gpx-parse.php gpxlog.gpx
* web-gui (http://[your router name here]/gps/) should show gpxlog.gpx
## configure cron
* copy line #1 from /tmp/mountd/disk1_part1/.cron/crontab
* crontab -e
* add copied line and remove #
## more testing
* /etc/init.d/gpsd restart
* logread -e gpx && date
* repeat until cron starts gpxlogger-cron.sh
## sms support
* send "Report GPS-Position" to GL-X3000 (exact typing pls)
### extract the phone number of incoming message
* find /etc/spool/sms/incoming/ -type f -exec grep -iE "^from: " {} \\;
* pick your number and add to /tmp/mountd/disk1_part1/scripts/tools/sms-check.masters
+ the message should be answered when cron job runs
## upload gpx-track's
### prepare the remote website
* configure your server to serve /tmp/mountd/disk1_part1/.www/upload/upload.php
* generate ca and user certificates
### configure local service
* edit /tmp/mountd/disk1_part1/.www/gps/RemoteConfig.php
### testing
* ./scripts/gpx-upload.php
### configure cron
* copy line #2 to crontab