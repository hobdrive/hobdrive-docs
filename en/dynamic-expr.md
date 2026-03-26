# Dynamic Expressions in Configuration Files

HobDrive includes an expression evaluator that greatly increases the flexibility of layout descriptions and sensor display formatting.

To use expressions in gauge and layout files, a special attribute notation is used.

For example, `precision='${1+2}'` will be evaluated as `precision='3'`

## Variables

The evaluator supports variables, allowing HobDrive to create flexible descriptions adapted, for example, to different color themes.
Variables can be used in two forms: $name or ${name}.

At startup, HobDrive defines groups of variables available in all layout/gauge files:

$daynight - value is day or night - the current operating mode.
$layout - value is portrait or landscape - the current screen orientation.
$Color_normal - the color group of the current theme. In addition to normal, $Color_back, $Color_contrast, $Color_minor, and $Color_subtle are also available.
$Theme_brightness - A group of variables starting with $Theme_; the full list can be found in the theme file. $Theme_brightness takes the value light or dark, allowing you to differentiate between light and dark themes.

Variables can also be defined within a gauge file itself:

`<variable id="Color_red" value="${color(lerp(Color_normal; 0xFF0000; 0.7))}"/>`

This creates a variable Color_red whose value is a red color blended toward the current text color. For light themes, this will be a dark red / maroon; for dark themes, it will be a light red / pink.


Example
` <gauge id="TireSensorLF,TireSensorRF,TireSensorLR,TireSensorRR" precision="1"
 color-map="${Tires_normal_pressure-2}:$Color_red, ${Tires_normal_pressure}:$Color_green, ${Tires_normal_pressure+2}:$Color_red"/>`

This defines tire pressure sensors. Their color depends on the color map (color-map).
Tires_normal_pressure is a variable with a default value of 32 (PSI).
${Tires_normal_pressure-2}:$Color_red becomes 30:#FF0000, meaning pressure of 30 PSI and below will be drawn in red.
${Tires_normal_pressure}:$Color_green - pressure of 32 PSI will be drawn in green.
${Tires_normal_pressure+2}:$Color_red - pressure of 34 PSI and above will be drawn in red again.

## Dynamic Interpolation

Core synax is documented here: [Dynamic expressions Core Syntax](dynamic-expr-core.md)

The interpolation method described above using a single $ is static. The system evaluates the data once when loading layout files. After loading, these expressions are frozen and become plain text.

However, in some sensor and decorator descriptions you can use dynamic interpolation. The difference is that it is evaluated every time the sensor is rendered.

Dynamic interpolation starts with a double $$ sign.

For example:


`<item id="DistanceRun_primary" precision="3"
              text-evaluator='${Sensor_Text}(gas), ${svaluef("DistanceRun_secondary"; 3)} (petrol)'
              description="hidden"/>`

Here the text-evaluator attribute on a sensor is used to create custom text for displaying the sensor value. There are several functions and variables that apply only to such dynamic interpolators.

Sensor_Text - the current text representation of the sensor. This is the text that would normally be displayed if no interpolator were present.

svaluef - this function takes the value of an arbitrary sensor from its argument and formats it, potentially converting units of measurement to the appropriate display units. It can be used as svaluef("YourSensor", 3) to explicitly specify the number of decimal places for formatting.

svaluef_units - same as above, but the unit of measurement will be displayed next to the value.

svalid - whether the sensor specified in the argument is valid (Boolean value).

svalue - the numeric (double) value of the sensor without unit conversion.

## Hobdrive Layout functions

HobDrive extends the list of functions with its own:

color - converts a number representing an RGB color into a form understood by HobDrive: #RRGGBB
For example, color(0x804020) returns #804020 - red (128), green (64), blue (32).
lerp(c1; c2; amount) - shifts the color given by c1 toward color c2, by the amount (from 0 to 1).
For example, lerp(Color_normal; 0xFF0000; 0.7) shifts the original color in the Color_normal variable (text color) toward the red range (0xFF0000) by more than half (0.7).
Note that the result of lerp is a number, and for HobDrive to correctly interpret it as a color, you need to write color(lerp(....)).
svalue('Speed')

The current value of the specified sensor. Note that widget updates occur only when the primary sensor specified in the id attribute changes.

svalue_ex('FuelEconomy', 'day')

  The value of the sensor's extended options.

sexists('Speed')

  True if the sensor exists in the current configuration.

svalid('Speed')

  True if the sensor exists and has a valid read value.

svaluef('LitersPerHour'; 2)

The current value as text with specified precision (number of decimal places).

Returns formatted sensor value in display units with specified precision.

svaluef_units("Sensor"; 1)

Returns formatted sensor value with units suffix and with specified precision.

t('SomeText')

Translates the specified key using HobDrive's localization system.
