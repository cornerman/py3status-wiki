Py3status (since v2) also comes with some configurable modules you can **load and configure** directly from your i3status.conf just like any other i3status module.

## Available modules
See the sources of each modules to know all its the configuration parameters.

* [This is the list of all available modules](https://github.com/ultrabug/py3status/tree/master/py3status/modules)

## Loading a py3status module
To load a py3status module you just have to **list it like any other i3status module** using the `order +=` parameter.

For example you could insert and load the `imap` module like this :

    order += "disk /home"
    order += "disk /"
    order += "imap"
    order += "time"

## Ordering modules output
The example above speaks for itself. Ordering your py3status modules in your i3bar is just the same as i3status modules, just list the **order** parameter where you want your module to be displayed.

## Configuring a py3status module
Your py3status modules are configured the exact same way as i3status modules, directly from your i3status.conf, like this :

    # configure the py3status imap module
    # and run thunderbird when I left click on it
    imap {
        cache_timeout = 60
        imap_server = 'imap.myprovider.com'
        mailbox = 'INBOX'
        name = 'Mail'
        password = 'coconut'
        port = '993'
        user = 'mylogin'
        on_click 1 = "exec thunderbird"
    }
