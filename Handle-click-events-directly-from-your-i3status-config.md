Py3status (since v2) is **wrapping and extending your i3status.conf** and allows you directly handle **all** the i3bar click events on **any** of your configured modules whether they are i3status modules or py3status modules.

## on_click parameter
All you have to do is add a new configuration parameter named `on_click [button number]` to your module config and py3status will then execute the given i3 command (using i3-msg).

This means you can run simple tasks like executing a program or execute any other i3 specific command.

As an added feature and in order to get your i3bar more responsive, **every on_click command will also trigger a module refresh**. This works for both py3status modules and i3status modules as described in the **refresh** command below.

## special on_click commands
There are two commands you can pass to the on_click parameter that have a special meaning to py3status :

- **refresh** : This will refresh (expire the cache) of the clicked module. This also works for i3status modules (it will send a SIGUSR1 to i3status for you).
- **refresh_all** : This will refresh **all** the modules from your i3bar (i3status included). This has the same effect has sending a SIGUSR1 to py3status.

Please note that for obvious abuse (click spam) protection, those commands are rate limited to 1 per second.

## using i3bar_click_events module ?
All `i3bar_click_events` module users are advised to migrate away from this special module and use the new `on_click` parameter explained in this page. The `i3bar_click_events` module will still work and be used as a fallback to the new `on_click` parameter if it is loaded.

This means that, for each module, **the `on_click` parameter overrides the click events configured in the `i3bar_click_events` module**.

## Examples
Some examples below from i3status.conf :

    # reload the i3 config when I left click on the i3status time module
    # and restart i3 when I middle click on it
    time {
        on_click 1 = "reload"
        on_click 2 = "restart"
    }

    # control the volume with your mouse (need >i3-4.8)
    # launch alsamixer when I left click
    # kill it when I right click
    # toggle mute/unmute when I middle click
    # increase the volume when I scroll the mouse wheel up
    # decrease the volume when I scroll the mouse wheel down
    volume master {
        format = "♪: %volume"
        device = "default"
        mixer = "Master"
        mixer_idx = 0
        on_click 1 = "exec i3-sensible-terminal -e alsamixer"
        on_click 2 = "exec amixer set Master toggle"
        on_click 3 = "exec killall alsamixer"
        on_click 4 = "exec amixer set Master 1+"
        on_click 5 = "exec amixer set Master 1-"
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

    # this is a py3status module configuration
    # open an URL on opera when I left click on the weather_yahoo module
    weather_yahoo paris {
        cache_timeout = 1800
        city_code = "FRXX0076"
        forecast_days = 2
        on_click 1 = "exec opera http://www.meteo.fr"
        request_timeout = 10
    }