`py3status` allows you to easily write your own modules and have their output displayed on your i3bar. You can even choose where the output will be placed in your bar.

## How it works
When called with the `-i PATH` parameter, `py3status` does the following :
* Find any file within the directory with a `.py` extension
* Check each file for a `Py3status` class
* Import each method of the `Py3status` class
* For every iteration, defined by the `-n INTERVAL` parameter (default 1sec), **call every method which was imported previously**
* Each method is passed two arguments : the **json** (dict) representing the modules' results to be displayed on your bar and the ***i3status_config*** (dict) representing the i3status parsed configuration variables
* Each method must return a **tuple** containing the **index** at which the output must be inserted in the final json and a i3bar-protocol compatible dict representing the output of your module.

## Internal caching
There is an internal cache layer on every user's module output controlled by the `-t CACHE_TIMEOUT` parameter (default 60 sec). This is meant as a convenience so you don't have to implement it yourself on every class you want included and to preserve your system performance.
* You can disable it by setting `-t 0`.
* You can **force your own cache timeout for a given module** by adding a `cached_until` key containing an epoch from time.time()

## Example class
You can find an example class in the `examples` folder named **empty_class.py**
