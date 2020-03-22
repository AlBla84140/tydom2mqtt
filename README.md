# tydom2mqtt
Link a Delta Dore's Tydom hub to a mqtt broker, working flawlessly here for days with the help of systemd's restarting mechanism.
The script works in background, with remote mode by default (see todo), and rollback in local mode if necessary (offline mode).
Disconnections from both servers are handled with automatic reconnections.

Not based on a poll mechanism but on push from websocket, i.e. it constantly pings the tydom hub to keep alive the connection and receive updates instantly.

Home assistant users : Use MQTT auto discovery, no further config necessary ! (Hassio docker install + MQTT addon on top of Ubuntu server is my personal setup.)
For other home automation systems you just add devices like any MQTT devices, check on the cover and alarm py files to see or modify the topics !

Based on https://github.com/cth35/tydom_python

Recommanded setup : any Linux OS with systemd management and python3.7

Needed packages : "pip3 install websockets requests gmqtt sdnotify"

- Add your credentials to main.py
- Put all the *.py files in a folder of your choice (Hassio users : by default i've put them in the share folder of hassio for backup purposes "/usr/share/hassio/share/tydom/")
- Put the tydom.service file in the folder of your choice (/usr/lib/systemd/system/ if you don't know where)
- Change the path of the main.py file in tydom.service
- with commandline, do "systemctl enable tydom.service" // It will start on boot, it's a set and forget !
- with commandline, do "systemctl start tydom.service"
- with commandline, check "systemctl status tydom.service" or with journalctl.

- To force a restart from your system, publish anything to hassio/tydom/kill, i will exit the script, systemd will restart it clean.

Alarm is readonly for now, (i've soldered a remote to optocouplers and an ESP8266 with esphome to set it from home assisant, a least it's very reliable !).

i don't have climate or lights to test, feel free to help !

TODO (but i've got no more time...) :

- Fix that annoying code 1006 systematic deconnection after 60s when using local mode (new since december, not before).
- Fix parsing of cdata msg type (will not crash anymore in the meantime), coming from an action from a alarm remote (and probably other things), we can get which remote had an action on alarm with it.
- Add all the attributes as json for home assistant (can see defect, etc. on attributes)
- Isolate parser in a class maybe...
- HACS / Docker versions
- Fork it to a proper Home Assistant integration with clean onboarding
- Add climate, lights, etc.
- Build a web frontend to see and test (use frontail for now)
- Add a folder of images to document where to solder on alarm remote and ESPHome configuration.
