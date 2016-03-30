Py3status (since v2) also comes with some configurable modules you can **load and configure** directly **from your i3status.conf** just like any other i3status module.

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

### Universal Module Options
As with i3status modules, you can configure [universal module options](http://i3wm.org/i3status/manpage.html#_universal_module_options) for each py3status module. These options are added to the response of the corresponding module and are then interpreted by [i3bar](https://i3wm.org/docs/i3bar-protocol.html#_blocks_in_detail). Currently the ```align```, ```min_width```, ```separator``` and ```separator_block_width``` options are supported. Example:

    # configure the py3status window_title_async module
    window_title_async {
        align = center
        min_width = 240
        separator = false
        separator_block_width = 20
    }

## Nota bene for py3status v1.x users
In order to respect the behavior of previous versions of py3status, all the modules present in inclusion folders (loaded by the `-i` command line flag) will be loaded automatically **unless you start using the new order += method described in this article**.

This means that once a py3status module is listed in the `order +=` parameters of your i3status.conf, this new mechanism **will take precedence over the legacy behavior**.
