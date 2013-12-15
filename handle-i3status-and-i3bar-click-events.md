`py3status` v1.1 offers a special module named **i3bar_click_events.py** which permits you to easily take action when clicking on **any** module displayed on your i3bar. **This means that you can handle your i3status modules click events !**

# Usage
First, you need to enable the `py3status` module which you can get from the _examples_ directory.
* If you don't use your own modules yet, create the folder _~/.i3/py3status/_
* Then copy the **i3bar_click_events.py** file in this directory (or the one you're using to load your current modules from)

# Configuration
Now all you have to do is edit the copied **i3bar_click_events.py** module. There are quite a lot of comments in it so it should be really simple but we'll sum it up here with an example usage.

## Basics
The idea is that you have to define a python dict key for each i3status/i3bar module and then specify which action to do on the desired button. This is done by setting up this variable:

        self.actions = {
            "<module name and instance>": {
                <button number>: [<function to run>, <arg1>, <arg2>],
            }
        }

## Example usage
* We want to open **wicd-gtk** when LEFT clicking on our i3status **ethernet eth0** and **wireless wlan0** modules and close it when RIGHT clicking on them
* We want to open **emelfm2** when LEFT clicking on our i3status **disk /** module
* We want to start **openVPN** when LEFT clicking on our i3status **run_watch /var/run/openvpn.pid** and stop it when RIGHT clicking on it

## Resulting configuration

        self.actions = {
            "ethernet eth0": {
                1: [external_command, 'wicd-gtk', '-n'],
                3: [external_command, 'killall', 'wicd-client'],
            },
            "wireless wlan0": {
                1: [external_command, 'wicd-gtk', '-n'],
                3: [external_command, 'killall', 'wicd-client'],
            },
            "disk_info /": {
                1: [external_command, 'emelfm2', '-1', '/'],
            },
            "run_watch /var/run/openvpn.pid": {
                1: [external_command, 'sudo', '/etc/init.d/openvpn', 'start'],
                3: [external_command, 'sudo', '/etc/init.d/openvpn', 'stop'],
            },
        }
