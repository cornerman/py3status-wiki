## About
You will love `py3status` if you're using [i3wm](http://i3wm.org/) and are frustrated by the i3status [limitations](https://faq.i3wm.org/question/459/external-scriptsprograms-in-i3status-without-loosing-colors/) on your i3bar such as:
* you cannot hack into it easily
* you want more than the built-in modules and their limited configuration
* you cannot pipe the result of one of more scripts or commands in your bar easily
* you want to see the clock seconds increment in real time !

These limitations are there by design as said in the [i3status](http://i3wm.org/i3status/) man page:

> In general, i3status wants to display things which you would look at occasionally anyways, like the current date/time, whether you are connected to a WiFi network or not, and if you have enough disk space to fit that 4.3 GiB download.

> However, if you need to look at some kind of information more than once in a while (like checking repeatedly how full your RAM is), you are probably better off with a script doing that, which pops up an alert when your RAM usage reaches a certain threshold. After all, the point of computers is not to burden you with additional boring tasks like repeatedly checking a number.

> In i3status, we don’t want to implement process management again. Therefore, there is no module to run arbitrary scripts or commands. Instead, you should use your shell.

## Philosophy & goals
* **no extra configuration file needed**
* **rely on i3status**' strengths and its **existing configuration** as much as possible
* **be extensible**, it must be easy for users to add their own stuff/output by writing a simple python class which will be loaded and executed dynamically
* add some **built-in enhancement/transformation** of basic i3status modules output

## Example
This is an example screenshot of my current i3bar using py3status :
![i3bar with py3status](http://ultrabug.fr/github/i3bar.png)

* I am using a color-enabled i3status with the standard `py3status` transformations, the colors are meaningful enough to spare the ": yes/no" of the _run-watch_ modules, also the clock ticks for every second without calling i3status
* I injected the result of a Py3status class printing the number of open tickets on our local GLPI system.

## Add you own stuff
See the [write your own modules](https://github.com/ultrabug/py3status/wiki/Write-your-own-modules) wiki page for more information.

## Handle i3status click events (and more)
See the [handle i3status and i3bar click events](https://github.com/ultrabug/py3status/wiki/handle-i3status-and-i3bar-click-events) wiki page for more information.