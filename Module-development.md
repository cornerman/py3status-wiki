## Write your own modules !
`py3status` allows you to easily write your own modules and have their output displayed on your i3bar. You can even choose where the output will be placed in your bar.

## How it works
When called with the `-i PATH` parameter, `py3status` does the following :
* Find any file within the directory with a `.py` extension
* Check each file for a `Py3status` class
* Import each method of the `Py3status` class
* For every iteration, defined by the `-n INTERVAL` parameter (default 1sec), call every method which was imported previously
* Each method is passed a single argument : the **json** (dict) representing the modules' results to be displayed on your bar
* Each method must return a **tuple** containing the **index** at which the output must be inserted in the final json and a i3bar-protocol compatible dict representing the output of your module.
