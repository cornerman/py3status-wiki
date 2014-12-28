Py3status (since v2) is **wrapping and extending your i3status.conf** and allows you directly handle **all** the i3bar click events on **any** of your configured modules whether they are i3status modules or py3status modules.

## on_click parameter
All you have to do is add a new configuration parameter named `on_click [button number]` to your module config and py3status will then execute the given i3 command (using i3-msg).

This means you can run simple tasks like executing a program or execute any other i3 specific command.

## Examples
Some examples below from i3status.conf :

    # reload the i3 config when I left click on the i3status time module
    # and restart i3 when I middle click on it
    time {
        on_click 1 = "reload"
        on_click 2 = "restart"
    }

    # run wicd-gtk GUI when I left click on the i3status ethernet module
    # and kill it when I right click on it
    ethernet eth0 {
        # if you use %speed, i3status requires root privileges
        format_up = "E: %ip"
        format_down = ""
        on_click 1 = "exec wicd-gtk"
        on_click 3 = "exec killall wicd-gtk"
    }

    # run thunar when I left click on the / disk info module
    disk / {
        format = "/ %free"
        on_click 1 = "exec thunar /"
    }

    # open an URL on opera when I left click on the py3status weather_yahoo module
    weather_yahoo paris {
        cache_timeout = 1800
        city_code = "FRXX0076"
        forecast_days = 2
        on_click 1 = "exec opera http://www.meteo.fr"
        request_timeout = 10
    }
