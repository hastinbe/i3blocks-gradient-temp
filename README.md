gradient-temp
=========
[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)][license]

A temperature indicator for [i3blocks] and [polybar], colored according to heat.

![](example.png)

## Installation

#### Requirements
* awk (POSIX compatible)
* bc
* [i3blocks] or [polybar]

#### Optional
* fonts-font-awesome

### Command-line options
```
Usage: ./gradient-temp [<options>] <command> [<args>]

Display a temperature indicator using lm_sensors for i3blocks or polybar.

Commands:
  query <sensor>            query sensor
  help                      display help

Options:
  -b <type>     set bar type: i3blocks (default) or polybar
  -H            display temperatures in fahrenheit
  -G            disable color gradient
  -c <temp>     override critical temperature
  -m <temp>     override maximum temperature
  -f <font>     set font for text and symbols
  -t <color>    set color when gradient is disabled
  -S <#color>   set gradient start color (hex triplet)
  -E <#color>   set gradient end color (hex triplet)
  -C <symbol>   set celsius symbol
  -F <symbol>   set fahrenheit symbol
  -T <feature>  set temperature subfeature (default: temp1)
  -h            display this help and exit
```

### Guide
Copy the blocklet configuration in the given `i3blocks.conf` into your [i3blocks] configuration file, the recommended config is:

```INI
[gradient-temp]
label=&#x1f321;
markup=pango
command=$SCRIPT_DIR/gradient-temp
interval=10
instance=__REPLACE_ME__

#FONT=FontAwesome
#GRADIENT=true
#FAHRENHEIT=false
#COLOR_TEMP=white
#COLOR_GRADIENT_START=#0000FF
#COLOR_GRADIENT_END=#FF0000
#SYMBOL_C=&#x2103;
#SYMBOL_F=&#x2109;
#OVERRIDE_CRIT_TEMP=90
#OVERRIDE_MAX_TEMP=100
```

Set the `instance` variable to the sensor to monitor, see `sensors -u`.

### Polybar Configuration

For polybar, add the following to your polybar config file:

```INI
[module/temperature]
type = custom/script
exec = /path/to/gradient-temp query __REPLACE_ME__
interval = 10
format-prefix = "🌡 "
format-prefix-foreground = ${colors.foreground-alt}

# Optional: Set bar type to polybar
# BAR_TYPE=polybar

# Optional configuration variables:
# GRADIENT=true
# FAHRENHEIT=false
# COLOR_TEMP=white
# COLOR_GRADIENT_START=#0000FF
# COLOR_GRADIENT_END=#FF0000
# SYMBOL_C=°C  (HTML entities like &#x2103; are automatically converted for polybar)
# SYMBOL_F=°F  (HTML entities like &#x2109; are automatically converted for polybar)
# OVERRIDE_CRIT_TEMP=90
# OVERRIDE_MAX_TEMP=100
```

Or use environment variables in your polybar config:

```INI
[module/temperature]
type = custom/script
exec = BAR_TYPE=polybar /path/to/gradient-temp query __REPLACE_ME__
interval = 10
```

Replace `__REPLACE_ME__` with the sensor to monitor (see `sensors -u`).

## Customization

The following can be set in your i3blocks.conf in newer versions if [i3blocks], in older versions (<= 1.4-4) you can customize using the program options. The following are configurable:

Variable | Type | Option | Description
------------ | ------------ | ------------- | -------------
BAR_TYPE | string | -b &lt;type&gt; | Bar type: i3blocks (default) or polybar
FONT | string | -f &lt;font&gt; | Font for text and symbols (i3blocks only)
SENSOR | string | -s &lt;sensor&gt; | The sensor to read
GRADIENT | boolean | -G | Whether or not to use the color gradient
FAHRENHEIT | boolean | -H | Whether or not to use fahrenheit
COLOR_TEMP | string | -t &lt;color&gt; | Color of text when not using the color gradient
COLOR_GRADIENT_START | string | -S &lt;#color&gt; | Start color for gradient; Must be a hex triplet prefixed with #
COLOR_GRADIENT_END | string | -E &lt;#color&gt; | End color for gradient; Must be a hex triplet prefixed with #
SYMBOL_C | string | -C &lt;symbol&gt; | Symbol for celsius
SYMBOL_F | string | -F &lt;symbol&gt; | Symbol for fahrenheit
OVERRIDE_CRIT_TEMP | float | -c &lt;temp&gt; | Override critical temperature
OVERRIDE_MAX_TEMP | float | -m &lt;temp&gt; | Override maximum temperature

## License

`gradient-temp` is released under [GNU General Public License v2][license]

Copyright (C) 1989, 1991 Free Software Foundation, Inc.

[i3blocks]: https://vivien.github.io/i3blocks/
[polybar]: https://github.com/polybar/polybar
[license]: https://www.gnu.org/licenses/gpl-2.0.en.html
