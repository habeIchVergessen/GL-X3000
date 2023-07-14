# GL-X3000

monitor gpsd, gpxlogger

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
* copy line 1 from /tmp/mountd/disk1_part1/.cron/crontab
* crontab -e
* add copied line and remove #
## more testing
* /etc/init.d/gpsd restart
* logread -e gpx && date
* repeat until cron starts gpxlogger-cron.sh