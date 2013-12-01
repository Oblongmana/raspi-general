# General notes on stuff to do with raspi tinkering

Aim: without using a graphical interface (other than on OSX to get the Raspbian image onto the sd card), get Raspberry pi basics set up, including bluetooth and wifi capabilities. Aim here is to use this headless in a single location (that is, network would need reconfiguring if network details changed)

## Gear

 1. Raspberry Pi (Model B)
 2. Ethernet cable (+ internet connection, obviously?)
 3. Cirago Bluetooth/WiFi dongle
 4. MicroUSB cable + wall charger

## Initial setup

### Get image

 1. Download/unzip raspbian image
 2. Download/install PiFiller
 3. (Optional) Plug in SD Card, name it something sensible
 4. Run PiFiller, installing raspbian onto SD Card
 5. So far, so good

### Basic Raspberry Pi setup

 1. Plug SD Card in
 2. Plug ethernet in
 3. Use whatever magic you need to use to find out IP address for RasPi (e.g. log into router and view ARP table)
 4. `ssh pi@[ip.add.re.ss]`: default password is raspberry
 5. `sudo raspi-config`
 6. Do whatever you need to do to expand the root filesystem (look for something like `expand_rootfs` - may vary depending on version of raspbian)
 7. Do other fun things like change your timezone, and your password
 8. Exit config
 9. Reboot (you should get prompted - if not, type `reboot` at the prompt)
 10. Once rebooted, `ssh pi@[ip.add.re.ss]`
 11. `sudo apt-get update`
 12. `sudo apt-get dist-upgrade`, or just `upgrade` if you know the difference between the two and prefer using `upgrade` rather than `dist-upgrade`
 13. `sudo sync`
 14. `reboot`

### Bluetooth setup

 1. If it's plugged in, un-plug the Cirago dongle
 2. `ssh pi@[ip.add.re.ss]`
 3. `sudo apt-get install bluetooth bluez-utils bluez-compat`
 4. Plug in the Cirago dongle (or unplug it and plug it back in if you ignored previous instructions!)
 5. You may lose connection and need to ssh back in
 6. Make something bluetoothy discoverable (like your phone, for example. No, turning on bluetooth does not do that usually, you need to dig into the settings and make it discoverable)
 7. `hcitool scan` (you should see blue flashing lights on the dongle)
 8. Observe that your bluetoothy device is listed after a search

### WiFi Setup

 1. Plug in the Cirago dongle
 2. `ssh pi@[ip.add.re.ss]`
 3. `sudo apt-get install wicd wicd-cli wicd-curses`
 4. Check everything's looking good so far by doing `sudo iwlist scan` and checking that you can see your network (assuming it's not hidden, in which case skip this step!)
 5. `wicd-curses`
 6. Using up and down, highlight your network, then use the right arrow key
 7. Navigate to "Use Static IPs" section and press [Enter]
 8. Complete the following
 
    ```
    IP Address: [SOMETHING APPROPRIATE FOR YOUR NETWORK, e.g. 192.168.1.222]
    Netmask: 255.255.255.0
    Gateway: 192.168.1.1
    ```

 9. Navigate to "Use global DNS servers" and press [Enter]
 7. Navigate to "Automatically connect to this network" and press [Enter]
 8. Navigate to "Key" and enter in your network passphrase
 9. Press [F10]
 10. You should now be back at the list of available networks - hit [Shift+c] with your network highlighted
 11. Unplug the ethernet cable
 12. You will have lost connection, try SSHing back in (NB: your IP address may have changed). If it works, you're done!
 13. If it didn't work, it's probably (hopefully) because your router doesn't like wireless devices talking to each other (think about say a Starbucks wifi network - best you prevent devices talking to each other!)
 14. Log into your router's control panel (generally, navigate to 192.168.1.1 and enter your password in).
 15. Dig down into the router's wireless settings, and look for something like "AP Isolation" or "Allow devices to communicate with each other over wlan" or something else that suggests communication between two wirelessly connected devices
 16. Disable that setting and save. Your router will probably do something like restart or be unresponsive for a while - wait this out.
 17. SSH in
 18. Have a beer or other refreshing drink
 19. If it still doesn't work, sorry, I don't have any other ideas for you off the top of my head :(