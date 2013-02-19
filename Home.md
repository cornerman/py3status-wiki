You will love `py3status` if you're using [i3wm](http://i3wm.org/) and are frustrated by the [i3status](http://i3wm.org/i3status/) [limitations](https://faq.i3wm.org/question/459/external-scriptsprograms-in-i3status-without-loosing-colors/) on your i3bar such as:
* you cannot hack into it easily
* you want more than the built-in modules and their limited configuration
* you cannot pipe the result of one of more scripts or commands in your bar easily
* you want to see the clock seconds increment in real time !

These limitations are there by design as said in the i3status man page:

> In general, i3status wants to display things which you would look at occasionally anyways, like the current date/time, whether you are connected to a WiFi network or not, and if you have enough disk space to fit that 4.3 GiB download.

> However, if you need to look at some kind of information more than once in a while (like checking repeatedly how full your RAM is), you are probably better off with a script doing that, which pops up an alert when your RAM usage reaches a certain threshold. After all, the point of computers is not to burden you with additional boring tasks like repeatedly checking a number.

> In i3status, we don’t want to implement process management again. Therefore, there is no module to run arbitrary scripts or commands. Instead, you should use your shell.

# Philosophy
* Keep what i3status is good for and was designed to do (see above)
* Do **not** replace i3status but **extend** it instead so that we can preserve all of i3status' advantages and configuration
* It is very easy to start using `py3status` as **it won't interfere with your existing settings** !
* You can easily **write your own modules** and have them displayed on your i3bar automagically
* It is easy to modify the existing modules output