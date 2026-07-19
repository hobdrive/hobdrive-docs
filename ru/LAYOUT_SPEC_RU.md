# Спецификация языка разметки HobDrive

[Главная](../README.md) | [English](../en/LAYOUT_SPEC.md) | [Russian](../ru/LAYOUT_SPEC_RU.md)

Этот документ собирает теги, атрибуты и опции, используемые в файлах макетов (layout) интерфейса HobDrive

Цель: дать краткую и практичную справку для создания и редактирования UI-раскладок (панели, экраны ЭБУ, гибрид/GPS панели и т.д.).

## Содержание
{:.no_toc}
* TOC
{:toc}

## Контракт макета
- Вход: XML-подобные файлы layout (теги: `ui`, `section`, `grid`, `item`, `union`, `stack-vertical`, `stack-horizontal` и т.д.)
- Выход: отображаемые UI-экраны в HobDrive; условная видимость и декораторы влияют на поведение во время выполнения
- Режимы ошибок: неверные атрибуты игнорируются или используются значения по умолчанию; отсутствующие датчики отображаются пустыми/недоступными
- Критерии успеха: макет рендерится и учитывает условные атрибуты `if`, размещение в сетке, декораторы и атрибуты виджетов

## Верхнеуровневые теги
- `ui` — корневой элемент файла layout. Содержит элементы `section` и другие конструкции макета.

- `include` — позволяет встраивать другой файл layout или фрагмент. Атрибуты:
  - `file` — относительный путь к включаемому файлу (например, `file="default-tripcomp.layout"`).
  - `if` — опциональное условие, контролирующее, обрабатывается ли включаемый файл (та же синтаксис выражений, что и `section` `if`).
  - Инклюды обрабатываются при загрузке файла и обычно используются для составления главных макетов из переиспользуемых фрагментов (trip computer, HUD, dashkits).

Пример:
```xml
<include file="default-tripcomp.layout" if='${!sexists("GasTrigger")}'/>
```

## Файлы макетов и порядок загрузки

Обычный интерфейс HobDrive собирается из стандартных layout-файлов приложения и пользовательских переопределений.

Типичные стандартные файлы:

- `default-landscape.layout` — верхнеуровневый макет и общие `config`-параметры;
- `default-hobd.layout` — базовые секции HobDrive;
- подключаемые файлы вроде `default-tripcomp.layout`, `default-dashb3.layout`, ECU-специфичных layout-файлов.

Пользовательские файлы:

- `user1.layout` — экран, созданный диалогом **Ваш экран** в настройках главных экранов;
- `user.layout` — ручные пользовательские секции и переопределения;
- `user.gauge` — пользовательские настройки внешнего вида сенсоров;
- `override*` директории — изолированные наборы переопределений, в том числе установленные DashKits.

Стандартные `default-*.layout` и `default*.gauge` не стоит редактировать напрямую: они могут быть заменены при обновлении приложения. Для своих изменений используйте `user.layout`, `user1.layout`, `user.gauge` или DashKit.

`user.gauge` отличается от `user.layout`: HobDrive может автоматически изменять его из диалога настройки вида сенсора. Ручное редактирование возможно, но требует осторожности, чтобы не конфликтовать с настройками, сделанными через интерфейс.

## `section`
Определяет логический блок экрана/страницы. Общие атрибуты:
- `name` — человекочитаемое имя секции (используется в списках UI).
- `title` — отображаемый заголовок. Если не задан, используется `name`.
- `fixed` — `true/false` (зафиксирована ли секция в навигации). `fixed="true"` делает секцию частью основной панели экранов.
- `optional` — `true/false`. `optional="true"` оставляет секцию дополнительной: она не попадает в основное перелистывание, но может быть открыта через меню **Экраны**.
- `order` — числовой приоритет секции. `0` означает естественный порядок. Отрицательные значения помещают секцию перед обычными экранами, положительные — после ненумерованных экранов.
- `if` — условное выражение (имя датчика или выражение). Секция показывается только если условие истинно. Примеры: `if="Oxygen_b1s1"`, `if="PortraitLayout"`, `if="Renault_01_11103.ControlModuleVoltage"`.
- `class` — подобие CSS-класса, используемого системой для применения предопределённого поведения (например, `SensorList`).
- `ns` — namespace для фильтрации датчиков при использовании `SensorList`.
- `exclude` — regex для исключения ID датчиков в автогенерируемых списках сенсоров.
- `title-height` — высота области заголовка/верхнего отступа секции.
- `maximize-focus` — (из changelog) `true/false` убирает дополнительное управление при фокусе.
- `controls` — `autohide` или другие значения для управления видимостью кнопок UI.
- `volatile` — подсказка для временных/служебных секций.

Примеры:
```xml
<section name="GPS">
  ...
</section>
```

```xml
<section name="Fuel Trims" if="STFT1">
  ...
</section>
```

Навигационный пример:

```xml
<section name="My Diagnostics" title="Диагностика" optional="true" order="2" if="STFT1">
  <grid rows="," cols=",">
    <item id="STFT1"/>
    <item id="LTFT1"/>
  </grid>
</section>
```

### Видимость секций в приложении

В runtime у секции есть состояние `Optional`.

- `fixed="true"` выставляет `Optional=false`: экран входит в основную панель и доступен перелистыванием.
- `optional="true"` оставляет экран дополнительным.
- Пользовательские настройки главных экранов могут переопределить `optional` и `order`; они сохраняются в конфигурации как `LayoutSection_<SectionName>_Optional` и `LayoutSection_<SectionName>_Order`.
- Секции с именем, начинающимся на `@`, используются как служебные действия в меню.

Если секция должна быть доступна только при конкретном датчике, ECU или ориентации, используйте `if`.

Частые условия:

```xml
<section name="Only ATF" if="ATFTemp">
  ...
</section>

<section name="Portrait Tools" if="PortraitLayout">
  ...
</section>

<section name="Landscape Tools" if="LandscapeLayout">
  ...
</section>
```


### `config` элементы
- `config` — объявляет конфигурируемые в UI опции, которые появляются в диалогах настроек. Типичные атрибуты:
  - `id` — уникальный идентификатор, доступный в выражениях макета (например, `${Layout_ECUFuel}`).
  - `type` — `bool`, `int`, `string` и т.д.
  - `default` — значение по умолчанию.
  - `title`, `descr` — текст, отображаемый в UI; поддерживаются локализованные варианты вроде `title-ru`, `descr-ru`.

Пример:
```xml
<config id="Layout_ECUFuel" type="bool" default="false"
        title="Use Fuel Level data from ECU"
        descr="If enabled - fuel level will be shown from ECU. Otherwise - estimated from fuel consumption"
        title-ru="Показывать уровень топлива из ЭБУ"
        descr-ru="Если включено - уровень топлива будет браться из ЭБУ. Иначе - показывается оценочный уровень топлива"/>
```

## Контейнеры макета
 - `grid` — размещает дочерние `item` в табличной сетке.
  - Атрибуты: `rows`, `cols` — значения, разделённые запятыми, задающие подсказки по размерам. Пример: `rows=",," cols=",35"`.
  - `rows` и `cols` могут включать численные высоты/ширины или пустые элементы для гибкой размерности.
  - Сетки можно вкладывать.

- `stack-vertical` / `stack-horizontal` — укладывают элементы вертикально/горизонтально; может использоваться `columns`.
  - Пример: `<stack-vertical columns="${LandscapeLayout ? 2 : 1}">`

- `union` — позволяет композировать несколько альтернативных макетов в одной области (используется в TPMS и т.д.).
  - Применяйте `union`, когда несколько взаимоисключающих подмакетов занимают одну и ту же область; каждый ребёнок обычно содержит свою `grid`.

- `switch` — выбирает один из дочерних элементов для показа на основе выражения `index`. Полезно для циклических списков или условных блоков.
  - Атрибуты:
    - `index` — выражение или целое число, выбирающее, какой ребёнок показать (0-based). Может ссылаться на значения сенсоров через `svalue(...)` или любые TinyExe-выражения.
    - `interval` — при наличии `index` может периодически пересчитываться с указанным интервалом (в миллисекундах) для циклирования детей.
    - `if` — опциональное условие для включения/выключения `switch` целиком.
  - Дети обычно `item` (или вложенные `grid`); когда `index` даёт номер ребёнка, показывается соответствующий элемент.

Пример (циклим температуры каждые 4 секунды):
```xml
<switch if='${Layout_RollTemp = true}' index='$${ svalue("DateTime") / 4000 }'>
  <item id="ATFTemp" .../>
  <item id="CVTTemp" .../>
  <item id="EngineOilTemp" .../>
</switch>
```

## `item`
Представляет виджет в `grid` или `stack`. Общие атрибуты и примеры использования:

- `id` — идентификатор датчика или специального управления. Примеры: `RPM`, `Latitude`, `ControlModuleVoltage`.
- `type` — `button`, `static` и пр. По умолчанию — виджет для отображения значения датчика.
- `text` — для кнопок или статических блоков — видимый текст.
- `action` — для `button`: строка действия, например `invoke(FAN_ON)` или `run(...)`.
- `size` — подсказка визуального размера: `micro`, `small`, `normal`, `large`, `huge`, `extralarge` или числовое значение.
- `precision` — количество знаков после запятой или специальные теги вроде `6fixed`.
- `inherit` — ссылается на имя декоратора/стиля, например `inherit="_ColoredOnOff"`.
- `colspan` / `rowspan` — расширение элемента на несколько ячеек сетки.
- `description` — дополнительная метка/описание (например `description="aside"`).
- `period` — период обновления в миллисекундах (например, `period='5000'`).
- `wrap` — фильтр для сглаживания/предсказания: примеры из changelog:
  - `wrap="predict(50,0.7)"`
  - `wrap="smooth(400,100,5)"`
  - `wrap="average(30)"`
- доп. атрибуты: `text-values`, `custom-units`, `colored-descr`, `text-evaluator`

- `onclick` — обработчик клика на элементе (навигация или запуск действия). Примеры: `onclick="NewFueling"`, `onclick="Go(Efficiency)"`.
- `actions` — список действий через запятую, доступных в диалоге информации о сенсоре. Пример: `actions="SetupOdometer"`.
- `units` — расположение текста единиц: `units="below"`, `units="aside"` или `units="hidden"`.
- `interval` — интервал обновления для динамики UI или внутренних таймеров (отличается от `period` датчика).
- `decorator-XXX` — специальный синтаксис для включения декораторов.

Примеры:
```xml
<item id="FuelLevel" if='${Layout_ECUFuel != true}' inherit="_FuelIndicator, _EditAction" onclick="NewFueling"/>
<item id="Odometer" if="${Layout_ECU_Odometer != true}" actions="SetupOdometer" size="large" inherit="_EditAction"/>
```

### Действия элементов

Действия можно привязать к элементам через `onclick`, `ondoubleclick`, `onhold`, `action` и `actions`.

Примеры:

```xml
<item id="FuelLevel" onclick="NewFueling"/>
<item id="Speed" onhold="SensorInformation"/>
<item type="button" text="Min" action="Minimize"/>
<item id="Speed" actions="SetupOdometer, Minimize, Shutdown"/>
<item id="EfficiencyPercentage" onclick="Go(Efficiency)"/>
```

Частые действия:

- `ChangeLiterCost` — диалог стоимости литра топлива;
- `FuelCalibration` — диалог настройки объема бака;
- `SensorInformation` — информация по сенсору;
- `NewFueling` — новая заправка;
- `NewMaintenance` — новая запись обслуживания;
- `Minimize` — скрыть приложение;
- `Shutdown` — выход;
- `VehicleSettings` — настройки автомобиля;
- `PortSettings` — настройки соединения;
- `ExtraSettings` — системные настройки;
- `EditSensorsLayout` — режим редактирования сенсоров;
- `CorrectionInformation` — информация по коррекции расхода;
- `Go(ScreenName)` или `go(ScreenName)` — переход на экран;
- `run(...)` — запуск внешнего действия/команды, если платформа это поддерживает.

- Специфика типов и атрибуты графиков:
  - `type="chart"` — рисует график по сенсору; общие атрибуты: `chart='line'`, `time-scale`, `min-limit`, `max-limit`, `cover-description`.
  - `border-opacity` — контроль непрозрачности рамки (используется в dashkits).
  - `custom-description` / `description` / `custom-units` — кастомные подписи и единицы.

- Доступ к extradata сенсора:
  - `id="sensor[extradata]"` — отображает дополнительные данные, связанные с сенсором (например `TripTime[fillup]`).

Примеры:
```xml
<item id='Latitude' precision='6fixed' size="normal"/>
<item type="button" text="Fan On" action="invoke(FAN_ON)" size='extralarge' />
<item id='Lambda' colspan="2"/>
```

## Специальные типы item
- `type="button"` — кликабельная кнопка. Используйте `action="invoke(NAME)"` для вызова ECU/сервиса.
- `type="static"` — статический текстовый блок. Пример: `type="static" size="micro"`
- gauge / chart / image decorators — могут быть объявлены как вложенные декораторы внутри `item` или в файлах тем/гауджей; могут принимать декораторы.

## Декораторы и вложенные элементы

`item` может содержать вложенные декораторы. Пример синтаксиса:

```xml
<item id="Speed">
  <image image-zorder="bottom" image-path="images/dashboard-svgrepo-com.svg" image-scale="0.5"/>
  <image image-zorder="bottom" image-path="images/arrow.png" image-scale="0.5"/>
</item>
```

Современная вложенная форма удобнее для новых макетов, потому что все параметры декоратора находятся рядом с `item`.

Также поддерживается старый атрибутный синтаксис `decorator-*`, который часто встречается в существующих `gauge` и `layout` файлах:

```xml
<item id="Speed"
      decorator-i1="image"
      i1-image-zorder="bottom"
      i1-image-path="images/gauges/without_arrow_circle.png"
      decorator-i2="image"
      i2-image-zorder="bottom"
      i2-image-path="images/gauges/arrow.png"
      i2-image-rotate='$${ Sensor_Value - 45 }'
      i2-image-scale="1"/>
```

Идентификатор после `decorator-` произвольный (`i1`, `bg`, `pad`, `action` и т.д.). Атрибуты декоратора задаются с тем же префиксом: `i1-image-path`, `pad-padding`, `action-image-align`.

Порядок наложения старых декораторов определяется сортировкой их идентификаторов. Для предсказуемого порядка удобно использовать числовые или алфавитные префиксы: `d1`, `d2`, `d3`.

### Декоратор `image`

Атрибуты: `image-zorder`, `image-path`, `image-scale`, `image-width`, `image-height`, `image-align`, `image-aspect`, `image-rotate`, `image-rotate-align` (могут содержать динамические выражения).

Многие атрибуты могут принимать динамические выражения, например `image-rotate='$${ Sensor_Value - 45 }'`.

Путь `image-path` обычно задается относительно папки ресурсов HobDrive или текущего DashKit. Для прозрачности используйте PNG или SVG.

Пример динамической картинки:

```xml
<item id="RPM">
  <image image-zorder="bottom"
         image-path='images/gauges/rpms/$${ceiling(Sensor_Value/1000*2)}.png'
         image-align="hcenter,bottom"
         image-scale="0.8"/>
</item>
```

### Декоратор progress

```xml
<item id="Speed" decorator-10="touch-progress"/>
```

Это системный декоратор, используется для отрисовки динамической линии при нажатии.

### Декоратор tile

```xml
<item id="Speed" decorator-10="tile"/>
```

Рисует плитку позади gauge.


### Декоратор padding

Декоратор padding используется для создания отступов в item.
Значения задаются для TOP, RIGHT, BOTTOM, LEFT.

Можно использовать процентные значения.

```xml
<item id="Speed">
    <padding padding="0 0 0 120"/>
</item>
```

### Декоратор crop

Декоратор crop обрезает (скрывает) часть ранее отрисованного виджета.

```xml
<item id="Test" border-opacity="1" type='roundbar' red-green="150,0">
  <crop crop="10% 10% 10% 10%"/>
</item>
```

### Декоратор visible / visibility

Этот декоратор просто включает/отключает отрисовку во время выполнения. Вы можете использовать динамическую оценку $$.

```xml
<item id="AT_R" visibility='$${Sensor_Value}'/>
```
### Декоратор filter

Декоратор filter — это мощный инструмент для применения визуальных эффектов и преобразований к элементам UI. Он поддерживает как эффекты отрисовки (визуальные фильтры), так и матричные преобразования (геометрические изменения).

#### Атрибуты

- `effect` — список функций фильтров, разделённых точкой с запятой. Каждый фильтр — это вызов функции с параметрами.
- `mode` — управляет тем, когда фильтр применяется в конвейере рендеринга:
  - `before` — применяет фильтр перед отрисовкой целевого элемента, затем рисует элемент нормально
  - `in` (по умолчанию) — применяет фильтр только к целевому элементу
  - `after` — сначала рисует целевой элемент, затем применяет фильтр
- `interval` — опционально; интервал обновления в миллисекундах для динамических выражений фильтра (с использованием синтаксиса `$${}`).

#### Эффекты отрисовки

Эти фильтры изменяют визуальный вид без изменения геометрии. Несколько фильтров можно объединить в цепочку с помощью точки с запятой.

**blur(radiusX, radiusY)**
- Применяет размытие по Гауссу к элементу
- `radiusX` — радиус размытия по горизонтали в пикселях
- `radiusY` — радиус размытия по вертикали в пикселях
- Пример: `effect="blur(2,2)"` — применяет размытие 2px в обоих направлениях

**drop-shadow(dx, dy, sigmaX, sigmaY)**
- Создаёт эффект тени
- `dx` — смещение тени по горизонтали в пикселях
- `dy` — смещение тени по вертикали в пикселях
- `sigmaX` — размытие тени по горизонтали
- `sigmaY` — размытие тени по вертикали (опционально, по умолчанию равно sigmaX)
- Цвет тени — чёрный
- Пример: `effect="drop-shadow(5,5,3,3)"` — тень смещена на 5px вправо и вниз с размытием 3px

**transparent(alpha)**
- Регулирует прозрачность элемента
- `alpha` — уменьшение непрозрачности (0.0 = полностью прозрачный, 1.0 = без изменений)
- Пример: `effect="transparent(0.5)"` — делает элемент на 50% более прозрачным

**dilate(radiusX, radiusY)**
- Расширяет светлые области (морфологическая дилатация)
- `radiusX` — радиус расширения по горизонтали в пикселях
- `radiusY` — радиус расширения по вертикали в пикселях
- Пример: `effect="dilate(2,2)"` — расширяет светлые области на 2px

**erode(radiusX, radiusY)**
- Сжимает светлые области (морфологическая эрозия)
- `radiusX` — радиус эрозии по горизонтали в пикселях
- `radiusY` — радиус эрозии по вертикали в пикселях
- Пример: `effect="erode(2,2)"` — сжимает светлые области на 2px

**brightness(factor)**
- Регулирует яркость элемента
- `factor` — множитель яркости (0.0 = чёрный, 1.0 = оригинал, >1.0 = ярче)
- Пример: `effect="brightness(1.5)"` — увеличивает яркость на 50%

**contrast(factor)**
- Регулирует контрастность элемента
- `factor` — множитель контрастности (0.0 = серый, 1.0 = оригинал, >1.0 = выше контрастность)
- Пример: `effect="contrast(1.3)"` — увеличивает контрастность на 30%

**invert()**
- Инвертирует цвета (эффект негатива)
- Без параметров
- Пример: `effect="invert()"` — создаёт цветовой негатив

**sepia()**
- Применяет сепию (тёплый коричневатый оттенок)
- Без параметров
- Пример: `effect="sepia()"` — эффект винтажной фотографии

**grayscale()**
- Преобразует в оттенки серого (на основе яркости)
- Без параметров
- Использует стандартные веса яркости (R: 0.2126, G: 0.7152, B: 0.0722)
- Пример: `effect="grayscale()"` — эффект чёрно-белой фотографии

**saturate(amount)**
- Регулирует насыщенность цвета
- `amount` — изменение насыщенности (-1.0 = обесцветить, 0.0 = без изменений, >0.0 = более насыщенный)
- Пример: `effect="saturate(0.5)"` — увеличивает насыщенность на 50%
- Пример: `effect="saturate(-0.3)"` — уменьшает насыщенность на 30%

**colorfilter(r, g, b)**
- Тонирует элемент определённым цветом, сохраняя яркость
- `r` — красный компонент (0-255)
- `g` — зелёный компонент (0-255)
- `b` — синий компонент (0-255)
- Преобразует изображение в оттенки серого, затем применяет цветовой оттенок
- Пример: `effect="colorfilter(255,100,50)"` — оранжевый оттенок

**remove-black()**
- Делает чёрные пиксели прозрачными (полезно для удаления чёрных фонов)
- Без параметров
- Альфа-канал становится пропорциональным яркости пикселя
- Пример: `effect="remove-black()"` — удаляет чёрный фон с изображений

#### Матричные преобразования

**Важно:** Матричные преобразования (rotate, offset, zoom) изменяют геометрию и не должны смешиваться с эффектами отрисовки в одном фильтре. Используйте отдельные декораторы `<filter>`, если вам нужно и то, и другое.

**rotate(angle, [pivotX, pivotY])**
- Поворачивает элемент
- `angle` — угол поворота в градусах
- `pivotX` — опционально; горизонтальная точка вращения (проценты или пиксели, по умолчанию: 50%)
- `pivotY` — опционально; вертикальная точка вращения (проценты или пиксели, по умолчанию: 50%)
- Пример: `effect="rotate(45)"` — поворачивает на 45° вокруг центра
- Пример: `effect="rotate(90,0%,0%)"` — поворачивает на 90° вокруг левого верхнего угла
- Динамический пример: `effect="rotate($${Sensor_Value})"` — поворачивает на основе значения датчика

**offset(dx, dy)**
- Сдвигает (перемещает) элемент
- `dx` — смещение по горизонтали (проценты или пиксели)
- `dy` — смещение по вертикали (проценты или пиксели)
- Пример: `effect="offset(10,20)"` — перемещает на 10px вправо, 20px вниз
- Пример: `effect="offset(50%,0)"` — перемещает на 50% ширины вправо

**zoom(scale)** или **zoom(scaleX, scaleY, [pivotX, pivotY])**
- Масштабирует элемент
- `scale` — равномерный коэффициент масштаба (1.0 = исходный размер)
- ИЛИ:
- `scaleX` — коэффициент масштаба по горизонтали
- `scaleY` — коэффициент масштаба по вертикали
- `pivotX` — опционально; горизонтальный центр масштабирования (проценты или пиксели)
- `pivotY` — опционально; вертикальный центр масштабирования (проценты или пиксели)
- Пример: `effect="zoom(1.5)"` — масштабирует до 150% вокруг центра
- Пример: `effect="zoom(2,1)"` — только удваивает ширину
- Пример: `effect="zoom(0.5,0.5,0%,0%)"` — масштабирует до 50% от левого верхнего угла

#### Примеры использования

Простой эффект размытия:
```xml
<item id="Speed">
  <filter effect="blur(2,2)"/>
</item>
```

Несколько объединённых в цепочку эффектов:
```xml
<item id="Temperature">
  <filter effect="grayscale();saturate(0.5);brightness(1.2)"/>
</item>
```

Динамическое вращение на основе значения датчика:
```xml
<item id="Compass">
  <filter effect="rotate($${Sensor_Value},50%,50%)" interval="100"/>
</item>
```

Использование режима с несколькими декораторами:
```xml
<item id="Speed" type="chart" chart='line'>
  <break/>
  <image image-zorder="bottom" image-path="images/ui/arrow.png"/>
  <filter effect="rotate($${180+Sensor_Value%360},50%,50%)" mode="in" interval="100"/>
  <filter effect="blur(1,1)" mode="after"/>
  <padding padding="0 0 0 120"/>
</item>
```

Цветовое тонирование:
```xml
<item id="Warning">
  <filter effect="colorfilter(255,0,0)"/>  <!-- Красный оттенок -->
</item>
```

#### Лучшие практики

- Объединяйте несколько эффектов отрисовки в один фильтр, используя точку с запятой, для лучшей производительности
- Не смешивайте матричные преобразования с эффектами отрисовки в одном атрибуте `effect`
- Используйте атрибут `interval` только когда у вас есть динамические выражения `$${}`, требующие периодического обновления
- Используйте `mode="in"` (по умолчанию) в большинстве случаев; используйте `mode="before"` или `mode="after"` для специальных задач наложения
- Сочетайте декоратор `<break/>` перед фильтрами, когда хотите применить эффекты только к последующим декораторам, а не к базовому элементу

### Декоратор break

Вы можете использовать break для прерывания наложения других декораторов.
Break означает, что конвейер отрисовки начинается с пустого.

В этом примере break используется для применения фильтров
только к декоратору image, а не к исходному
содержимому gauge "Speed" (значению и графику).

```xml
<item id="Speed" type="chart" chart='line'>
  <break/>
  <image image-zorder="bottom" image-path="images/ui/arrow-circle-right.png" image-scale="0.6"/>
  <filter effect="rotate($${180+Sensor_Value%360},50%,50%)" mode="in" interval="100"/>
  <filter effect="blur(1,1)"/>
  <padding padding="0 0 0 120"/>
</item>
```

## Уровневые атрибуты и функции (из changelog)
- `if` — условное отображение для `section` или `item`.
- `ignore-gauges` в `section` — игнорировать глобальные дескрипторы gauges для всех элементов внутри секции.
- `gauge` локальные определения могут объявляться внутри `section`.
- `swap-direction` на gauges: `swap-direction="true"` меняет направление бара или roundbar.
- `horizontal-offset` для roundbar.
- декоратор `break` — `<break/>` разделяет рендеринг на независимые этапы.
- `maximize-focus`, `controls="autohide"` — полезны для dash панелей.
- `time-scale` — используется для изменения шкалы времени в графиках.
- `text-evaluator` — расширенный атрибут для gauges, возвращающий динамический текст.

Дополнительные наблюдаемые атрибуты и поведения:
- `optional` / `volatile` на `section`.
- `period` на `item`.
- `precision`.
- `colspan` / `rowspan`.

Пример графика с фильтрами и кастомным описанием:
```xml
<item id="Speed" wrap="predict(50,0.7)" type="chart" chart='line' period="100" precision="0" custom-description="Predictive speed" border-opacity="1"/>
```

## Сенсоры и выражения
- Сенсоры могут ссылаться напрямую по `id` в `if` атрибутах или как `item`.
- TinyExe хелперы и `eval` выражения доступны в ecuxml. Доступные глобалы: Pi, E, PositiveInfinity, NegativeInfinity и пр.
- Фильтры сенсоров (через `wrap`) как выше.

Пример TinyExe / eval:

`image-rotate='$${ Sensor_Value - 45 }'`

Пример доступа к extradata:

`<item id="TripTime[fillup]"/>`

## `SensorList` и автогенерируемые панели
- `section` с `class="SensorList"` и `ns="<namespace>"` генерирует экран со списком датчиков для namespace. Используйте `exclude` regex для фильтрации шумных датчиков.
  Пример:
  ```xml
  <section name="Ford" class="SensorList" ns="Ford_MondeoDuratorq2003" if="Ford_MondeoDuratorq2003.Speed" exclude="\\bCACT_|...">
  </section>
  ```

## Примеры (взято из реальных layout)
- Простой GPS экран (из `ecu-gps.layout`):

```xml
<section name="GPS">
  <grid rows=",," cols=",,">
    <item id='Latitude' precision='6fixed' size="normal"/>
    <item id='Longitude' precision='6fixed' size="normal"/>
    <item id='Satellites'/>
    <item id='Altitude' precision='1'/>
    <item id='GPSSpeed' precision='1'/>
    <item id='ActiveSatellites'/>
    <item id='Direction'/>
    <item id='GPSOBD2SpeedDiff'/>
    <item id='FixedPosition'/>
  </grid>
</section>
```

- Fan control (кнопки + inherit decorator):

```xml
<section name="FAN" fixed="false" if="PortraitLayout">
  <grid rows=",," cols="">
    <item id="FAN_Status" inherit="_ColoredOnOff" size='large'/>
    <item type="button" text="Fan On" action="invoke(FAN_ON)" size='extralarge' />
    <item type="button" text="Fan Off" action="invoke(FAN_OFF)" size='extralarge' />
  </grid>
</section>
```

- Fuel trims:

```xml
<section name="Fuel Trims" if="STFT1">
  <grid rows="," cols=",">
    <item id="STFT1" size='large' precision="1" />
    <item id="STFT2" size='large' precision="1" />
    <item id="LTFT1" size='large' precision="1" />
    <item id="LTFT2" size='large' precision="1" />
  </grid>
</section>
```

## Условные выражения
- Атрибут `if` может быть простым тестом присутствия сенсора (например `if="STFT1"`) или полным выражением, ссылающимся на переменные темы, флаги день/ночь, флаги макета и значения сенсоров.

Примечание: `if` может содержать через запятую несколько идентификаторов, которые в практике работают как OR (множество вариантов).

## Интерполяция в выражениях

Кроме простых выражений доступны два стиля интерполяции:

- `${ }` интерполяция: вычисляется один раз при сборке макета. Используйте для статических вычислений.

- `$${ }` интерполяция: вычисляется динамически — подходит для значений, которые меняются во времени (анимации, периодические обновления сенсоров). Применимо к выбранным атрибутам: `text-evaluator`, `switch[index]`, `visibility`, `image-rotate`, `image-path`, `crop`.

Эти формы позволяют вставлять TinyExe-выражения в значения атрибутов для статического или динамического вычисления.

## Подсказки по размерам сетки
- `rows` и `cols` принимают значения, разделённые запятыми. Пустые поля — гибкие ячейки, числа — фиксированные подсказки. Пример: `rows="30,,," cols="30,,30"`.

Можно вкладывать `grid` для создания сложных адаптивных областей.

## Дополнительные полезные атрибуты и поведения
- `period` на `item` — устанавливает период обновления виджета.
- `precision` — точность отображения.
- `description="aside"` — размещает описательный текст сбоку.
- `inherit` — повторно использует стиль/цвета из темы.
- `text-values` и `custom-units` — показывают текстовые значения или альтернативные единицы.
- `min-limit` / `max-limit` — ограничивают значения на графиках.

## Widget и атрибуты декораторов (из `user.layout`)

- `type` (примеры): `bar`, `roundbar`, `chart`, `text`, `static`.

- `cache-interval` — подсказка кэширования/обновления UI. Пример: `cache-interval="6"`.

- `red-green` — список порогов цвета, значения через запятую. Примеры: `red-green="100,5"`.

- `bars-count` — число сегментов в bar/roundbar. Пример: `bars-count="10"`.

- `transparent` — `true/false`.

- `border-width` / `border-opacity` — визуальные параметры рамки.

- `colored-value` — применять окраску значений по порогам.

- `fade-coeff` — коэффициент затухания трейла в графиках.

- `width-compression` — сжатие по горизонтали.

- `item-size` / `item-width` / `item-height` / `item-spacing` — параметры размера сегментов.

- `small-radius-delta` / `small-radis-delta` — варианты опечатки встречались в исходниках; означают смещение малого радиуса.

- `start-angle` / `end-angle` / `radius` / `vertical-offset` — геометрические параметры roundbar.

- `corn-angle` / `middle-angle` — дополнительные угловые параметры.

- `decorator-p` (padding) и `p-padding` — декоратор отступов. Пример: `decorator-p="padding" p-padding="10 10 10 10"`.

- `decorator-i` / `decorator-iN` — нумерованные декораторы изображений (`decorator-i1="image"`, `i1-image-path`, `i1-image-rotate`, и т.д.). Альтернативно поддерживается вложенная форма `<image>`.

Примеры:
```xml
<item id="Speed" decorator-i1="image" i1-image-zorder="bottom" i1-image-path="images/gauges/without_arrow_circle.png" i1-image-rotate='$${...}' i1-image-rotate-align="center"/>

<item id="Speed">
  <image zorder="bottom" path="images/gauges/without_arrow_circle.png"/>
  <image zorder="bottom" path="images/gauges/arrow.png" rotate='$${...}' scale="1"/>
</item>
```

- `rotate` / `scale` на вложенных `<image>` — `rotate` может содержать динамические выражения, `scale` задаёт масштаб изображения.

- `custom-units` — используется для `DateTime` и пр.: `custom-units="time12"`, `time`, `timesec`, `datetime`, `seconds`.

- `border-opacity` — применялось в тестах для просмотра рамок.

- числовой `size` на `item` — можно задавать числа (например, `size="72"`) помимо именованных размеров.

## Частые `type` в dashkits
- `type="text"` — текстовое представление датчика (`text-values`).
- `type="chart"` — временной ряд.
- `type="button"` — интерактивная кнопка.

Пример использования `text-values`:

`<item id="RPM" wrap="predict(50, 0.7)" text-values="0:" type="text" description="hidden">`

## Advanced: ecuxml и определения сенсоров
- определения сенсоров и `eval` выражения находятся в ecuxml файлах. Примеры возможностей:
  - `<eval>` выражения для вычисляемых сенсоров
  - `base-data` / `base-raw` для сырых байтов
  - функции `get(offset)`, `get_word()` и т.д.
  - тег `text` для строковых сенсоров

## Практические рекомендации
- Используйте `if` для модульности файлов макетов; макеты загружаются только для релевантных ЭБУ, но `if` дополнительно предотвращает пустые секции.
- Применяйте `SensorList` для генерации экранов со списками сенсоров и `exclude` для отбора.
- Для сложной визуализации предпочтительнее вложенные декораторы — они локальны и проще в управлении.
- Применяйте `wrap` фильтры для шумных сенсоров (GPS, speed, MAF) для плавного отображения.
- По возможности держите gauges локальными для секции, используя `ignore-gauges` и локальные `gauge` объявления.

## DashKit-пакеты

DashKit — это папка с метаданными, layout-файлами и ресурсами, которую HobDrive может скачать из GitHub и установить в пользовательскую директорию как отдельное переопределение.

Официальный репозиторий: [hobdrive/hobdrive-dashkits](https://github.com/hobdrive/hobdrive-dashkits).

Шаблон для своих пакетов: [hobdrive/template-dashkits](https://github.com/hobdrive/template-dashkits).

Минимальная структура:

```text
community/my-dashkit/
  info.json
  user.layout
  images/
  README.md
```

`info.json`:

```json
{
  "name": "My DashKit",
  "version": "1.0.0",
  "description": "Custom dashboard for HobDrive",
  "author": "Your Name"
}
```

Поддерживаемые поля `info.json`:

- `name` — имя пакета;
- `version` — версия;
- `author` — автор;
- `description` — описание;
- `exclude` — дополнительные файлы/папки, которые не нужно скачивать при установке;
- `repo`, `branch`, `path`, `url`, `localPath` — служебные поля, которые HobDrive добавляет или использует после загрузки.

`user.layout` внутри DashKit использует тот же синтаксис, что и обычный пользовательский `user.layout`. Изображения можно класть рядом в `images/` и ссылаться на них из `image-path`.

При установке HobDrive создает локальную папку вида:

```text
override-<DashKitName>-<repo>-<branch>/
```

Эта папка читается как пользовательское переопределение. Поэтому DashKit может добавлять собственные `user.layout`, изображения, темы и другие ресурсы, не изменяя стандартные файлы приложения.

В приложении DashKits открываются через **Настройки -> Настройки Дэш Панелей**. Источник задается как GitHub `owner/repo` и ветка/ref; значения по умолчанию: `hobdrive/hobdrive-dashkits`, `main`.

## Appendix — быстрый справочник по атрибутам
- section: `name`, `title`, `fixed`, `optional`, `order`, `if`, `class`, `ns`, `exclude`, `title-height`, `maximize-focus`, `controls`, `volatile`
- grid: `rows`, `cols`
- item: `id`, `type`, `text`, `action`, `onclick`, `ondoubleclick`, `onhold`, `actions`, `size`, `precision`, `inherit`, `colspan`, `rowspan`, `description`, `period`, `wrap`
- decorators: `image` (`image-path`, `image-scale`, `image-zorder`, `image-rotate`, `image-width`/`image-height`), `padding`, `tile`, `touch-progress`, `crop`, `filter`, `break`

## Try it — готовые шаблоны

GPS panel (минимальный)
```xml
<ui>
  <section name="GPS">
    <grid rows=",," cols=",,">
      <item id='Latitude' precision='6fixed' size="normal"/>
      <item id='Longitude' precision='6fixed' size="normal"/>
      <item id='GPSSpeed' precision='1' />
    </grid>
  </section>
</ui>
```

Fan control (кнопки + inherit)
```xml
<ui>
  <section name="FAN" fixed="false" if="PortraitLayout">
    <grid rows=",," cols="">
      <item id="FAN_Status" inherit="_ColoredOnOff" size='large'/>
      <item type="button" text="Fan On" action="invoke(FAN_ON)" size='extralarge' />
      <item type="button" text="Fan Off" action="invoke(FAN_OFF)" size='extralarge' />
    </grid>
  </section>
</ui>
```

Hybrid summary example
```xml
<ui>
  <section name="Hybrid">
    <grid rows="30,,," cols="30,,30">
      <item id="Efficiency_Battery" size='huge' />
      <item id="FuelEconomy_NoBattery" size='huge' />
      <item id="BatteryCurrent" />
      <item id="SoC" />
    </grid>
  </section>
</ui>
```

---

Файл переведён с сохранением терминов, имен атрибутов и примеров в исходном английском виде.
