`py3status` allows you to easily write your own modules and have their output displayed on your i3bar. You can even choose where the output will be placed in your bar.

## How it works
When called with the `-i PATH` parameter (can be used more than once for multiple inclusions), `py3status` does the following :
* Find any file within the directory with a `.py` extension
* Check each file for a `Py3status` class
* Import each method of the `Py3status` class excluding : private (starting with _) and special methods (such as @staticmethod/@classmethod/@property)
* For every iteration, defined by the `-n INTERVAL` parameter (default 1sec), **call every method** which was imported previously **except reserved methods** which are : `kill` and `on_click`
* Each method is passed two arguments : the **json** (dict) representing the modules' results to be displayed on your bar and the **i3status_config** (dict) representing the i3status parsed configuration variables
* Each method must return a **tuple** containing the **index/position** at which the output must be inserted in the final output and a i3bar-protocol compatible dict representing the output of your module.
* Upon exit, call the `kill` method of every module if implemented.

## Ordering your modules' output
There are two methods to order your own modules' output on your i3bar. Just keep in mind that they are executed one after the other alphabetically and their output is inserted at index 0 of a list (unless you changed this).
### setting the index value to your module's return tuple
This is the preferred method as it gives you total control of what gets showed. Let's take an example of `module_a.py`, `module_b.py` and `module_c.py`.
* `module_a.py` return tuple is (0, {'full_text' : 'module A', 'name' : 'modA'})
* `module_b.py` return tuple is (0, {'full_text' : 'module B', 'name' : 'modB'})
* `module_c.py` return tuple is (0, {'full_text' : 'module C', 'name' : 'modC'})

You get on your bar : `module C | module B | module A`.

Now to get `module A | module C | module B`, we'd modify the modules so that :
* `module_a.py` return tuple is (0, {'full_text' : 'module A', 'name' : 'modA'}) _first executed, first added to output_
* `module_b.py` return tuple is (1, {'full_text' : 'module B', 'name' : 'modB'}) _second executed, we want it at the right of A, so index is 1_
* `module_c.py` return tuple is (1, {'full_text' : 'module C', 'name' : 'modC'}) _third executed, we want it also at the right of A, so index is also 1_

### naming
Simply change their name in the inclusion directory. **They'll appear in reverse order of their naming** in your bar because of the behavior explained above.

## Internal caching
There is an internal cache layer on every user's module output controlled by the `-t CACHE_TIMEOUT` parameter (default 60 sec). This is meant as a convenience so you don't have to implement it yourself on every class you want included and to preserve your system performance.
* You can disable it by setting `-t 0`.
* You can **force your own cache timeout for a given module** by adding a `cached_until` key containing an epoch from time.time()

## Example class
You can find an example classes in the `examples` folder of the repository.
