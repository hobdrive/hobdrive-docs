# Динамические выражения в файлах настроек

в хобдрайв внедрен анализатор выражений, позволяющий сильно увеличить гибкость описаний раскладок, формат отображения сенсоров.

Для использования выражений в gauge и layout файлах, применяется специальная форма записи атрибутов.

Например `precision='${1+2}'` превратится в `precision='3'`


## Переменные

Анализатор поддерживает переменные, что позволяет в хобдрайве создавать гибкие описания, адаптированные например к разным цветовым темам.
Переменные можно использовать в двух видах - $name или ${name}.

Хобдрайв при загрузке определяет группы переменных, доступные во всех layout/gauge файлах:

$daynight - значение day или night - текущий режим работы.
$layout - значение portrait или landscape - текущая ориентация экрана
$Color_normal - группа цветов текущей темы. Помимо normal доступны $Color_back, $Color_contrast, $Color_minor, $Color_subtle.
$Theme_brightness - Группа переменных начинающихся с $Theme_, весь список можно найти в файле темы. Указанный $Theme_brightness принимает значения light или dark - и позволяет разделять светлые и темные темы.

Переменные можно определять и в самом gauge файле:

`<variable id="Color_red" value="${color(lerp(Color_normal; 0xFF0000; 0.7))}"/>`

Создает переменную Color_red, значение ее - красный цвет, приближенный к текущему цвету текста. Для светлых тем оформления это будет темнокрасный, бардовый, для темных тем -светлокрасный, розовый.


Пример
` <gauge id="TireSensorLF,TireSensorRF,TireSensorLR,TireSensorRR" precision="1"
 color-map="${Tires_normal_pressure-2}:$Color_red, ${Tires_normal_pressure}:$Color_green, ${Tires_normal_pressure+2}:$Color_red"/>`

Определяет сенсоры давления шин. Их цвет зависит от карты цветов (color-map).
Tires_normal_pressure - переменная, со значением по умолчанию 32 (бара).
${Tires_normal_pressure-2}:$Color_red, - превратится в 30:#FF0000 - что означает что давление 30бар и ниже должно рисоваться красным цветом.
${Tires_normal_pressure}:$Color_green - давление 32бар будет рисоваться зеленым цветом
${Tires_normal_pressure+2}:$Color_red - давление 34бар и выше будет рисоваться опять красным цветом.

## Динамическая интерпретация

Указанный выше способ интерполяции с одинарным $ является статическим. Т.е. система один раз анализирует данные при загрузке файлов раскладок. После загрузки эти выражения замораживаются и превращаются в обычный текст.

Но в некоторых описаниях сенсоров и декораторов можно использовать динамическую интерполяцию. Её отличие - она вычисляется каждый раз при отрисовке сенсора.

Такая интерполяция начинается с двойного знака $$

Например:


`<item id="DistanceRun_primary" precision="3"
              text-evaluator='${Sensor_Text}(газ), ${svaluef("DistanceRun_secondary"; 3)} (бенз)'
              description="hidden"/>`

Здесь атрибут text-evaluator в датчике используется чтобы создать спецфичный текст для отображения значения датчика. Есть несколько функций и переменных, применимых только к таким динамическим интерполяторам.

Sensor_Text - это текущее текстовое представление сенсора. То, какой текст рисуется обычно, если интерполятор отсутствует.

svaluef - это функция берет значение произвольного датчика из аргумента, и форматирует его значение, возможно преобразуя единицы измерения в нужные для отображения. Можно использовать как svaluef("YourSensor", 3) - для явного указания числа знаков после запятой при форматировании

svaluef_units - то же что и выше, но рядом со значением выведется единица измерения.

svalid - Валиден ли сенсор указанный в аргументе (Булево значение).

svalue - цифровое (double) значение датчика без преобразования единиц.

## Описание синтаксиса

The functionality of the tool is based on the implementation as used in Excel. Currently the expression evaluator supports the following features:

It can parse mathematical expressions, including support for the most commonly used functions,e.g.:
4*(24/2-5)+14

cos(Pi/4)*sin(Pi/6)^2
1-1/E^(0.5^2)

min(5;2;9;10;42;35)

The following functions are supported:

About Abs Acos And Asin Atan Atan2 Avg Ceiling Clear Cos Cosh Exp Fact Floor Format Help Hex If Floor Left Len Ln Log Lower Max Min Mid Min Not Or Pow Right Round Sign Sin Sinh Sqr Sqrt StDev Trunc Upper Val Var

Basic string functions:

"Hello " & "world"
"Pi = " & Pi
Len("hello world")

Boolean operators:

true != false
5 > 6 ? "hello" : "world"
If(5 > 6;"hello";"world")

Function and variable declaration

x := 42
f(x) := x^2
f(x) := sin(x) / cos(x) // declare new dynamic functions using built-in functions
Pi
E

Recursion and scope

fac(n) := (n = 0) ? 1 : fac(n-1)*n
// fac calls itself with different parameters
f(x) = x*Y
// x is in function scope, Y is global scope

Currently 5 datatypes are supported: double, hexidecimal, int, string and boolean. Note that integers (and hexadecimals also) are always converted to doubles when used in a calculation by default. Use the int() function to convert to integer explicitly.

The tool uses the following precedence rules for its operators:

1. ( ), f(x) Grouping, functions
2. ! ~ - + (most) unary operations
3. ^ Power to (Excel rule: that is a^b^c -> (a^b)^c
4. * / % Multiplication, division, modulo
5. + - Addition and subtraction
6. & concatenation of strings
7. < <= > >= Comparisons: less-than, ...
8. = != <> Comparisons: equal and not equal
9. && Logical AND
10. || Logical OR
11. ?: Conditional expression
10 := Assignment

Хобдрайв расширяет список функций своими собственными:

color - превращает число, задающее RGB цвет в форму понятную хобдрайву - #RRGGBB
например color(0x804020) вернет #804020 - красный (128), зеленый (64) синий (32)
lerp(c1; c2; amount) - сдвигает цвет заданный числом с1 в направлении цвета с2, на величину amount (от 0 до 1)
например lerp(Color_normal; 0xFF0000; 0.7) делает исходный цвет в переменной Color_normal (цвет текста) сдвинутым в красный диапазон (0xFF0000) больше чем наполовину (0.7)
Заметьте что результат lerp - число, и чтобы хобдрайв правильно интерпретировал его как цвет, нужно писать color(lerp(....))
svalue('Speed')

Текущее значение указанного датчика. Заметьте что обновления виджета происходят только при изменении основного датчика, указанного в атрибуте id.

svalue_ex('FuelEconomy', 'day')

  Значение расширенных опций датчика. 

sexists('Speed')

  True если датчик существует в текущей конфигурации.

svalid('Speed')

  True если датчик существует и имеет валидное прочитанное значение

svaluef('LitersPerHour'; 2)

Текущее значение в текстовом виде с указанием точности (числа знаков после запятой)

Returns formatted sensor value in display units with specified precision

svaluef_units("Sensor"; 1)

Returns formatted sensor value with units suffix and with specified precision

t('SomeText')

Средствами хобдрайва перевести указанный ключ
