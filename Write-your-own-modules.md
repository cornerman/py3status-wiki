`py3status` allows you to easily write your own modules and have their output displayed on your i3bar. You can of course choose where the output will be placed in your bar.

## How it works
When called with the `-i PATH` parameter (can be used more than once for multiple inclusions), `py3status` does the following :
* Find any file within the directory with a `.py` extension
* Check each file for a `Py3status` class
* Import each method of the `Py3status` class excluding : private (starting with _) and special methods (such as @staticmethod/@classmethod/@property)
* For every iteration, defined by the `-n INTERVAL` parameter (default 1sec), **call every method** which was imported previously **except reserved methods** which are : `kill` and `on_click`
* Each method is passed two arguments : the **i3s_output_list** (list of dict) representing **i3status modules' output** as a list which are to be displayed on your bar and the **i3s_config** (dict) representing **i3status parsed configuration** variables
* Each method must return an i3bar-protocol compatible **dict** response containing at least the **full_text** key representing the output to be displayed.
* Upon exit, call the `kill` method of every module if implemented

### remember !
* Do **NOT** use **print** on your modules : `py3status` will catch any output and discard it silently because this **would break your i3bar** (see [issue #20](https://github.com/ultrabug/py3status/issues/20) for details).
* Make sure you **catch any output from any external program you may call from your module**. Any output from an external program cannot be caught and silenced by `py3status` and will break your i3bar so please, redirect any stdout/stderr to /dev/null for example (see [issue #20](https://github.com/ultrabug/py3status/issues/20) for details).

## Filter i3status output
You can change any of the i3status modules' output by modifying the **i3s_output_list** (list of dict) parameter received by your module !

## Click events
Click events are available since **i3 4.6**. `py3status` allows you to easily implement them on your modules by adding a `on_click` method to your Py3status class.

### where to handle click events ?
Since py3status v2, you can [handle click events directly from your usual i3status.conf](https://github.com/ultrabug/py3status/wiki/Handle-click-events-directly-from-your-i3status-config). It is thus advised and more simple to handle them from there for non module related operations such as executing an external program or an i3 message.

### default behavior
If your module's class do not implement a `on_click` method, py3status will react to **middle mouse clicks** on your module's output. Using the middle click on your output will result in **forcing a refresh** of the given module's method ouput.

### implementing on_click
To react specifically to click events, just add a `on_click` method to your Py3status class. This method gets one extra parameter, the event sent by the i3bar as a json/dict object. Example of event :

`{'y': 13, 'x': 1737, 'button': 1, 'name': 'empty', 'instance': 'first'}`

Click events are dispatched to the correct module thanks to the **name** and **instance** (if implemented) parameters. `py3status` will find out which module is responsible for the clicked element and execute this module's `on_click` method with the `i3status_output`, `i3s_config` and the `event_json` parameters.

## Ordering your modules' output since py3status v2
Since py3status v2 wraps your i3status configuration it will also respect the order defined by the i3status **order +=** parameters. All you have to do is add your module using the **order += "my module"** in the order of your choosing just like you do with any other i3status modules.

## Ordering your modules' output before py3status v2 (legacy)
There are two methods to order your own modules' output on your i3bar.
* by setting the index position of your methods yourself
* by renaming your modules' files

### ordering, behind the hood
The main thing to understand is that your modules' output are - in an alphabetically respecting turn - inserted in a list based on the index position they returned.
* **the index position** parameter returned by your methods specify the position you're wishing your output to be placed in that list.
* **the first (top left) one element of a list is index 0**.
* if two methods specify the same index position, they'll be ordered alphabetically.
* before ordering takes place, the list is prepared so every possible index position already exists. **This ensures that if your module asks for index position 1, it will be inserted at the second position on the list**.
* if you specify an index position out of range, it will be displayed **at the end** of your i3bar

The list also contains i3status output jsons so **you can insert your modules' output in between** i3status'. Let's see a prepared list example before ordering kicks in:

`[ '', '', '63°', '2013-08-30 00:00:00' ]`

This means:
* we have two modules (two blank fields at the start)
* i3status has two modules enabled (temperature and clock)

If we specified this:
* `module_a.py` return tuple is (1, {'full_text' : 'module A', 'name' : 'modA'})
* `module_b.py` return tuple is (0, {'full_text' : 'module B', 'name' : 'modB'})

We'd get this displayed:
`[ 'module B', 'module A', '63°', '2013-08-30 00:00:00' ]`

### setting the index value to your module's return tuple
This is the preferred method as it gives you total control of what gets showed and where. Let's take an example of `module_a.py`, `module_b.py` and `module_c.py`.
* `module_a.py` return tuple is (0, {'full_text' : 'module A', 'name' : 'modA'})
* `module_b.py` return tuple is (0, {'full_text' : 'module B', 'name' : 'modB'})
* `module_c.py` return tuple is (0, {'full_text' : 'module C', 'name' : 'modC'})

You get on your bar : `module A | module B | module C`.

Now to get `module A | module C | module B`, we'd modify the modules so that :
* `module_a.py` return tuple is (0, {'full_text' : 'module A', 'name' : 'modA'})
* `module_b.py` return tuple is (2, {'full_text' : 'module B', 'name' : 'modB'})
* `module_c.py` return tuple is (1, {'full_text' : 'module C', 'name' : 'modC'})

### naming
Simply change their name in the inclusion directory and make sure all your modules' methods returns an index position of 0 (or at least the same position). **Your modules will appear in alphabetical order of their naming** in your bar.

## Internal caching
There is an internal cache layer on every user's module output controlled by the `-t CACHE_TIMEOUT` parameter (default 60 sec). This is meant as a convenience so you don't have to implement it yourself on every class you want included and to preserve your system performance.
* You can disable it by setting `-t 0`.
* You can **force your own cache timeout for a given module** by adding a `cached_until` key containing an epoch from time.time()

## Debugging
As you can see in the **empty_class** module, you should be able to call your module directly from a shell using your python interpreter :

> python /path/to/module/empty_class.py

Example output :

> {'full_text': '', 'cached_until': 1419790914.257796}

You can also run `py3status` with the `--debug` parameter to be able to monitor your loaded modules and their output. `py3status` uses the standard **syslog** module so the actual logs are dispatched by your prefered syslog daemon to the **user** facility.
* On Gentoo Linux using metalog : `/var/log/everything/current`
* On Gentoo Linux using rsyslog : `/var/log/user.log`
* On Arch Linux : `/var/run/user/${UID}/i3` *(thx to @ShadowPrince)*

Example output :

> [py3status] module weather_yahoo.py click_events=False has_kill=False methods=dict_keys(['weather_yahoo'])

> [py3status] method weather_yahoo returned {'full_text': '☁ ☂ ☂ ☂', 'cached_until': 1383864691.163709}

## Example class
You can find an example class in the [py3status/modules](https://github.com/ultrabug/py3status/tree/master/py3status/modules) folder of the repository, look at the **empty_class.py** file.
