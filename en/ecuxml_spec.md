# ECUXML Format Specification

ECUXML is an XML-based format used by HOBDrive to define vehicle ECU (Electronic Control Unit) sensor parameters. Each `.ecuxml` file describes a set of OBD2/KWP2000 diagnostic parameters — how to request data from a vehicle's ECU, how to parse the raw response bytes, and how to present the resulting values to the user.

ECUXML files are compiled into C# sensor provider classes at build time by the `ecuxml2cs` tool, or loaded dynamically at runtime from external files.

---

## Table of Contents

{:.no_toc}
* TOC
{:toc}

---

## File Structure

### Root Element: `<parameters>`

Every ECUXML file has a single root element `<parameters>`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<parameters namespace="MyECU"
            description="My ECU description"
            description-ru="Описание ЭБУ"
            standard-header="7E1">
  <!-- global elements and parameter definitions -->
</parameters>
```

#### Root Attributes

| Attribute | Required | Description |
|-----------|----------|-------------|
| `namespace` | No | Sensor namespace prefix. All parameter IDs within this file will be prefixed with `Namespace.` unless the ID already contains a dot. Defaults to `"Default"`. |
| `description` | No | Short English description of this ECU profile. Shown in profile selection UI. |
| `description-ru` | No | Short Russian description. |
| `standard-header` | No | Default ELM327 header (`ATSH`) applied to all OBD2 sensors in this file, unless overridden per-parameter. |

### Global Elements

These child elements of `<parameters>` configure the ECU profile as a whole. They must appear before `<parameter>` elements.

#### `<include>`

Includes another ECUXML file. The included file's parameters are loaded into the same sensor registry. Paths are relative to the current file.

```xml
<include>../kwp2000.ecuxml</include>
<include>IM-FAN.ecuxml</include>
```

Multiple `<include>` elements are allowed.

#### `<header>`

Default ELM327 header string. Applied to all OBD2 sensors unless overridden. This is an alternative to the `standard-header` attribute on the root element.

```xml
<header>7E1</header>
```

Can also contain extended AT commands separated by semicolons:

```xml
<header>ATSH773; ATCRA774; ATFCSH773; 1092;</header>
```

#### `<obd2-header>`

Optional OBD2 default header. When present, this value is appended to the ELM327 initialization string. It is also used to restore the header back to default after a sensor with a custom header has been read.

```xml
<obd2-header>7E0</obd2-header>
```

Or with extended commands:

```xml
<obd2-header>ATSH7E0; ATCRA7E8; ATFCSH7E0;</obd2-header>
```

#### `<init-string>`

ELM327 initialization commands, separated by semicolons. Sent to the adapter when this ECU profile is activated.

```xml
<init-string>
  ATSP6; ATAL; ATSH7E0; ATST32;
</init-string>
```

#### `<disable-obd2>`

Controls whether standard OBD2 sensors are loaded alongside this file's custom sensors.

```xml
<disable-obd2>false</disable-obd2>
```

- `"false"` → standard OBD2 sensors **are** used (i.e. `UseOBD2 = true`)
- any other value → standard OBD2 sensors are **disabled**

When disabled, only the sensors defined in this ECUXML file (and its includes) are available.

#### `<models>`

Comma or newline-separated list of vehicle manufacturer names. Used for grouping in the ECU selection UI.

```xml
<models>
  Mitsubishi
</models>
```

```xml
<models>Peugeot, Citroen</models>
```

#### `<description lang="...">`

Detailed description of the ECU profile, shown in the UI. The `lang` attribute specifies the language (`"en"` or `"ru"`).

```xml
<description lang="en">
  Extra OBD2 sensors for turbocharger diagnostics.
</description>
<description lang="ru">
  Дополнительные датчики OBD2 для диагностики турбокомпрессора.
</description>
```

---

### Parameter Element: `<parameter>`

Each `<parameter>` defines a single sensor. The `id` attribute is required and serves as the sensor's identifier within its namespace.

```xml
<parameter id="CoolantTemp" display="PID05">
  <!-- child elements defining the sensor -->
</parameter>
```

#### Parameter Attributes

| Attribute | Description |
|-----------|-------------|
| `id` | **Required.** Unique sensor identifier. The final sensor ID becomes `Namespace.id` unless the id already contains a dot. |
| `display` | Optional display alias. Used by some ECU profiles as an alternative reference code (e.g., Renault `PR003`, Toyota `PID1`). |

---

## Sensor Types

The type of sensor created depends on which child elements are present within `<parameter>`. The parser checks for these in a specific priority order:

### 1. Direct OBD2 Sensor

A sensor that directly queries the ECU with a command (mode+PID or raw command) and parses the response bytes.

**Created when:** No `<base>`, `<base-raw>`, `<base-data>`, `<class>`, or `<text>` element is present.

```xml
<parameter id="CoolantTemp">
  <mode>1</mode>
  <command>05</command>
  <valuea>1</valuea>
  <offset>-40</offset>
  <description>
    <name>Coolant Temperature</name>
    <unit>celsius</unit>
  </description>
</parameter>
```

Or using `<address>` shorthand (OBD2 Mode 1):

```xml
<parameter id="Lambda11" display="LAMBDA11">
  <address><byte>0x34</byte></address>
  <valueab>0.0000305</valueab>
</parameter>
```

Or using a raw command string:

```xml
<parameter id="Core03">
  <raw>2103</raw>
  <valuea>0</valuea>
</parameter>
```

### 2. Derived Sensor

A sensor whose value is computed from one or two other sensors.

**Created when:** `<base>` is present and `<average>` is not.

```xml
<parameter id="TC_CINP">
  <base>IntakeManifoldPressure</base>
  <valuea>0.01</valuea>
  <offset>0</offset>
  <description>
    <name>TC Pressure</name>
    <unit>bar</unit>
  </description>
</parameter>
```

With two base sensors and an eval expression:

```xml
<parameter id="CombinedValue">
  <base>SensorA</base>
  <base2>SensorB</base2>
  <eval>Sensor_Value * 2 + Sensor2_Value</eval>
</parameter>
```

**Variables available in `<eval>` for derived sensors:**
- `Sensor_Value` — current value of the `<base>` sensor
- `Sensor2_Value` — current value of the `<base2>` sensor (if present)

### 3. Base-Raw Derived Sensor

A sensor that reads raw response bytes (including all headers) from another OBD2 sensor and extracts a value at a specific byte offset.

**Created when:** `<base-raw>` is present and `<text>` / `<dump>` are not.

```xml
<parameter id="CVTdegr">
  <base-raw>Core10</base-raw>
  <word-31>1</word-31>
  <offset>0</offset>
</parameter>
```

The `getraw()` family of methods is used — offsets are absolute positions in the raw byte array (including any protocol headers).

**Variables available in `<eval>` for base-raw sensors:**
- `Sensor` — the base OBD2 sensor object. Use `get()`, `get_word()`, etc. functions to access data.

### 4. Base-Data Derived Sensor

A sensor that reads data bytes from another OBD2 sensor, skipping the standard protocol headers.

**Created when:** `<base-data>` is present (and `<base>` / `<base-raw>` are not).

```xml
<parameter id="TireSensor1">
  <base-data>Core_21A8</base-data>
  <eval>get(15)*0.2</eval>
  <description>
    <unit>psi</unit>
  </description>
</parameter>
```

The `get()` family of methods is used — offsets are relative to the start of the data payload (after mode+command bytes).

**Variables available in `<eval>` for base-data sensors:**
- `Sensor` — the base OBD2 sensor object. Use `get()`, `get_word()`, etc. functions.

### 5. Text Sensor

A sensor that returns a text string from the ECU response rather than a numeric value (e.g., VIN code, ECU name).

**Created when:** `<text>` element is present.

As a direct OBD2 text sensor:

```xml
<parameter id="VIN">
  <mode>9</mode>
  <command>2</command>
  <text/>
</parameter>
```

As a derived text sensor from raw data:

```xml
<parameter id="ECUID_ECUIDT">
  <raw>1A80</raw>
  <text/>
</parameter>
```

As a derived text sensor from another sensor's raw data with eval:

```xml
<parameter id="StatusText">
  <base-data>CoreSensor</base-data>
  <text/>
  <eval>... expression returning a string ...</eval>
</parameter>
```

#### `<text>` Attributes

| Attribute | Description |
|-----------|-------------|
| `offset` | Byte offset within the response to start reading text. |
| `length` | Maximum number of bytes to read. |

### 6. Dump Sensor

Similar to text sensor but produces a hex dump of raw bytes. Useful for diagnostic display of raw data.

**Created when:** `<dump>` element is present with a `<base-data>` source.

```xml
<parameter id="ID_TireSensor_1">
  <base-data>TireSensorID</base-data>
  <dump offset="1" length="4"/>
</parameter>
```

#### `<dump>` Attributes

| Attribute | Description |
|-----------|-------------|
| `offset` | Byte offset within the data to start dumping. |
| `length` | Number of bytes to dump. |

### 7. Average Sensor

A sensor that computes a running average of another sensor's values over a specified number of readings.

**Created when:** `<average>` element is present along with `<base>`.

```xml
<parameter id="Avg_ECUFuelLevel">
  <base>ECUFuelInTank</base>
  <average length="15"/>
  <description>
    <unit>liters</unit>
  </description>
</parameter>
```

#### `<average>` Attributes

| Attribute | Description |
|-----------|-------------|
| `length` | Number of values to average over (integration window size). |

### 8. Class-Based Sensor

A sensor implemented by a named C# class rather than ECUXML value extraction logic.

**Created when:** `<class>` element is present.

```xml
<parameter id="MIL">
  <class>KWPMILSensor</class>
</parameter>
```

```xml
<parameter id="ClearDTC">
  <class>hobd.ClearDTCSensor</class>
</parameter>
```

The class must be registered in the sensor registry and inherit from `CoreSensor`.

---

## Value Extraction

### Command Specification

There are three ways to specify what command to send to the ECU:

#### Mode + Command (Standard OBD2)

```xml
<mode>1</mode>        <!-- OBD2 Mode, in hex (e.g., 1 = live data) -->
<command>0C</command>  <!-- OBD2 PID, in hex -->
```

Default mode is `1` if not specified.

#### Address shorthand

Equivalent to specifying mode 1 + the given PID:

```xml
<address><byte>0x34</byte></address>
```

#### Raw command

A raw hex string sent to the ELM327. Semicolons in the string are converted to carriage returns (multiple commands). This overrides mode+command.

```xml
<raw>2103</raw>
<raw>14FF00</raw>
<raw>1A80</raw>
```

### `<header>`

Per-parameter header override. Changes the ELM327 header (`ATSH`) for this sensor's requests, overriding both `standard-header` and the global `<header>`.

```xml
<parameter id="SpecialSensor">
  <header>7E1</header>
  <raw>2201</raw>
  <valuea>1</valuea>
</parameter>
```

### `<data-offset>`

Overrides the default command length for response parsing. Normally, the parser skips `CommandLength` bytes (default: 2, for Mode+Command) after finding the reply marker. This element changes that skip size.

```xml
<data-offset>2</data-offset>  <!-- skip 2 bytes after reply marker -->
<data-offset>3</data-offset>  <!-- skip 3 bytes (e.g., for 3-byte request commands) -->
```

### Single-Byte Values

Extract a single byte from the response data and multiply by the scale factor.

| Element | Reply Offset | Description |
|---------|-------------|-------------|
| `<value>` or `<valuea>` | 0 | First data byte (byte A) |
| `<valueb>` | 1 | Second data byte (byte B) |
| `<valuec>` | 2 | Third data byte (byte C) |
| `<valued>` | 3 | Fourth data byte (byte D) |
| `<value-N>` | N | Byte at offset N |

The element content is the **scale factor** (multiplier):

```xml
<valuea>0.392157</valuea>   <!-- result = byte_A * 0.392157 -->
<valueb>1</valueb>           <!-- result = byte_B * 1 -->
<value-6>1</value-6>         <!-- result = byte_at_offset_6 * 1 -->
```

### Word Values (16-bit)

Extract a 16-bit big-endian word (two consecutive bytes) and multiply by scale.

| Element | Reply Offset | Description |
|---------|-------------|-------------|
| `<valueab>` | 0 | Word from bytes A,B (offset 0,1) |
| `<valuebc>` | 1 | Word from bytes B,C (offset 1,2) |
| `<valuecd>` | 2 | Word from bytes C,D (offset 2,3) |
| `<word-N>` | N | Word starting at offset N |

```xml
<valueab>0.0000305</valueab>  <!-- result = word(A,B) * 0.0000305 -->
<word-31>1</word-31>           <!-- result = word at offset 31 -->
```

### DWord Values (32-bit)

Extract a 32-bit big-endian double word (four consecutive bytes) and multiply by scale.

| Element | Reply Offset | Description |
|---------|-------------|-------------|
| `<dword-N>` | N | DWord starting at offset N |

```xml
<dword-0>0.001</dword-0>  <!-- result = dword at offset 0 * 0.001 -->
```

### Little-Endian Variants

For ECUs that transmit data in little-endian byte order:

| Element | Description |
|---------|-------------|
| `<wordle-N>` | 16-bit little-endian word at offset N |
| `<dwordle-N>` | 32-bit little-endian double word at offset N |

```xml
<wordle-8>0.0018252</wordle-8>  <!-- LE word at offset 8 -->
```

### Value Transformation Pipeline

After extracting the raw numeric value, the following transformations are applied in order:

#### 1. Signed conversion (`<signed/>`)

If present, the extracted value is reinterpreted as a signed integer:
- Byte values: treated as `sbyte` (-128 to 127)
- Word values: treated as `short` (-32768 to 32767)
- DWord values: treated as `int`

```xml
<signed/>
```

#### 2. Scale and Offset

```
result = raw_value * scale + offset
```

Where `scale` comes from the value element content and `offset` from the `<offset>` element.

```xml
<valuea>1</valuea>
<offset>-40</offset>   <!-- e.g., Coolant Temp: byte_A * 1 + (-40) -->
```

#### 3. Bit extraction (`<bit>` and `<bit-width>`)

If `<bit>` is specified, the result is right-shifted by the given number of bits, then masked to extract `<bit-width>` bits (default: 1).

```xml
<bit>10</bit>          <!-- extract bit 10 -->
<bit-width>3</bit-width> <!-- extract 3 bits -->
```

Formula: `result = (int(scaled_value) >> bit) & ((1 << bit_width) - 1)`

This is commonly used to extract boolean flags or small fields from status bytes:

```xml
<parameter id="FuelSystemStatus_Trim">
  <base>FuelSystemStatus</base>
  <value>1</value>
  <bit>10</bit>
</parameter>
```

#### 4. Cut filters (`<cut-low>` and `<cut-high>`)

Clamp invalid/noisy values to zero:

```xml
<cut-low>500</cut-low>    <!-- values below 500 become 0 -->
<cut-high>220</cut-high>   <!-- values above 220 become 0 -->
```

- `<cut-low>`: if result < threshold, result = 0
- `<cut-high>`: if result > threshold, result = 0

Useful for filtering ECU bugs (e.g., phantom RPM readings when engine is off):

```xml
<parameter id="RPM">
  <base>RPM</base>
  <value>1</value>
  <cut-low>500</cut-low>
</parameter>
```

### MIL Group (`<mil-group>`)

For MIL (Malfunction Indicator Lamp) sensors, specifies which group of diagnostic trouble codes this sensor belongs to:

```xml
<mil-group>TPMS_Suzuki</mil-group>
<mil-group>HyundaiSRS</mil-group>
```

---

## Dynamic Expressions (`<eval>`)

The `<eval>` element contains a dynamic expression that computes the sensor value. When present, it overrides the standard value extraction (scale/offset) pipeline.

```xml
<eval>get(13)*0.2</eval>
<eval>(0.000000002344*(get(13)^5))+(-0.000001387*(get(13)^4))+(0.0003193*(get(13)^3))+(-0.03501*(get(13)^2))+(2.302*get(13))+(-36.6)</eval>
<eval>get(6)*65536+get(7)*256+get(8)</eval>
<eval>Sensor_Value * 2 + Sensor2_Value</eval>
```

### Expression Syntax

The expression evaluator is based on an Excel-like formula engine. It supports:

- **Arithmetic:** `+`, `-`, `*`, `/`, `%`, `^` (power)
- **Comparison:** `<`, `<=`, `>`, `>=`, `=`, `!=`, `<>`
- **Logical:** `&&`, `||`, `!`
- **Ternary:** `condition ? true_value : false_value`
- **String concatenation:** `&`
- **Assignment:** `:=`
- **Grouping:** `( )`

#### Operator Precedence (highest to lowest)

| Priority | Operators | Description |
|----------|-----------|-------------|
| 1 | `( )`, `f(x)` | Grouping, function calls |
| 2 | `!`, `~`, `-`, `+` | Unary operators |
| 3 | `^` | Power (left-associative: `a^b^c` = `(a^b)^c`) |
| 4 | `*`, `/`, `%` | Multiplication, division, modulo |
| 5 | `+`, `-` | Addition, subtraction |
| 6 | `&` | String concatenation |
| 7 | `<`, `<=`, `>`, `>=` | Comparison |
| 8 | `=`, `!=`, `<>` | Equality |
| 9 | `&&` | Logical AND |
| 10 | `\|\|` | Logical OR |
| 11 | `?:` | Ternary conditional |
| 12 | `:=` | Assignment |

### Data Types

Five data types are supported: `double`, `int`, `hexadecimal`, `string`, and `boolean`.

Integers and hexadecimals are automatically converted to `double` in calculations. Use `int()` to convert explicitly.

### Built-in Functions

The expression engine supports these standard functions:

`Abs`, `Acos`, `And`, `Asin`, `Atan`, `Atan2`, `Avg`, `Ceiling`, `Cos`, `Cosh`, `Exp`, `Fact`, `Floor`, `Format`, `Hex`, `If`, `Left`, `Len`, `Ln`, `Log`, `Lower`, `Max`, `Min`, `Mid`, `Not`, `Or`, `Pow`, `Right`, `Round`, `Sign`, `Sin`, `Sinh`, `Sqr`, `Sqrt`, `StDev`, `Trunc`, `Upper`, `Val`, `Var`

#### `If(condition; true_value; false_value)`

Conditional function. Equivalent to `condition ? true_value : false_value`.

Note: function arguments are separated by **semicolons** (`;`), not commas.

### ECUXML-Specific Functions

These functions are available **only** within `<eval>` expressions in ECUXML parameter definitions. They operate on the current sensor's raw data buffer.

#### `get(offset)`

Returns a single byte from the sensor's data buffer at the given offset. The offset is relative to the start of the data payload (after skipping the reply marker and command bytes).

```xml
<eval>get(0)</eval>          <!-- first data byte -->
<eval>get(13)*0.2</eval>     <!-- byte at offset 13, scaled -->
```

#### `get_word(offset)`

Returns a 16-bit big-endian word (two bytes) starting at the given offset.

```xml
<eval>get_word(8)*0.1</eval>
<eval>get_word(2)*0.005+2.1</eval>
```

#### `get_dword(offset)`

Returns a 32-bit big-endian double word (four bytes) starting at the given offset.

```xml
<eval>get_dword(0)*0.001</eval>
```

#### `get_wordle(offset)`

Returns a 16-bit little-endian word starting at the given offset.

```xml
<eval>get_wordle(0)*0.3144</eval>
```

#### `get_dwordle(offset)`

Returns a 32-bit little-endian double word starting at the given offset.

```xml
<eval>get_dwordle(0)</eval>
```

#### `to_signed_byte(value)` / `to_signed(value)`

Converts a byte value (0–255) to a signed byte (-128 to 127).

```xml
<eval>to_signed_byte(get(5))*0.5</eval>
```

#### `to_signed_word(value)`

Converts a 16-bit unsigned value to a signed 16-bit integer (-32768 to 32767).

```xml
<eval>to_signed_word(get_word(0))*0.1</eval>
```

#### `to_signed_dword(value)`

Converts a 32-bit unsigned value to a signed 32-bit integer.

### Eval context variables

Depending on the sensor type, different variables are available in eval expressions:

| Sensor Type | Available Variables |
|-------------|-------------------|
| Direct OBD2 (`<raw>` or `<mode>`/`<command>`) | `Sensor` — the OBD2 sensor object (use `get()`, `get_word()`, etc.) |
| Derived (`<base>`) | `Sensor_Value` — double value of the base sensor; `Sensor2_Value` — value of `<base2>` sensor |
| Base-raw (`<base-raw>`) | `Sensor` — the base OBD2 sensor object |
| Base-data (`<base-data>`) | `Sensor` — the base OBD2 sensor object |

---

## Description and Localization

Each parameter can have one or more `<description>` blocks for localization.

```xml
<parameter id="CVTTemp">
  <description>
    <name>CVT Temperature</name>
    <description>CVT oil temperature</description>
    <unit>celsius</unit>
    <display>CVTTEMP</display>
  </description>
  <description lang="ru">
    <name>Темп. CVT</name>
    <description>Температура масла вариатора</description>
    <unit>celsius</unit>
  </description>
</parameter>
```

### `<description>` Attributes

| Attribute | Description |
|-----------|-------------|
| `lang` | Language code (`"en"`, `"ru"`, etc.). If omitted, defaults to `""` (default/English). |

### `<description>` Child Elements

| Element | Description |
|---------|-------------|
| `<name>` | Human-readable sensor name. Shown in the UI. |
| `<description>` | Longer description / tooltip text. |
| `<unit>` | Measurement unit string (e.g., `celsius`, `bar`, `psi`, `km`, `volts`, `liters`, `rpm`, `cm`). HOBDrive uses this for unit conversion. |
| `<display>` | Display format override. |

---

## Complete Examples

### Example 1: Simple OBD2 Sensor with Mode/Command

Standard OBD2 coolant temperature (PID 0x05):

```xml
<parameter id="CoolantTemp">
  <address><byte>0x05</byte></address>
  <valuea>1</valuea>
  <offset>-40</offset>
  <description>
    <name>Coolant Temperature</name>
    <unit>celsius</unit>
  </description>
</parameter>
```

Value formula: `byte_A * 1 + (-40)` → temperature in °C.

### Example 2: Derived Sensor with Cut Filter

Filtering noisy RPM values:

```xml
<parameter id="RPM">
  <base>RPM</base>
  <value>1</value>
  <cut-low>500</cut-low>
</parameter>
```

Takes the existing RPM sensor value, passes it through (scale=1), but returns 0 if below 500.

### Example 3: Base-Data with Eval Expression

TPMS pressure from a multi-byte response:

```xml
<parameter id="TireSensor1">
  <description>
    <unit>psi</unit>
  </description>
  <base-data>Core_21A8</base-data>
  <eval>get(15)*0.2</eval>
</parameter>
```

Reads byte at offset 15 from `Core_21A8`'s data payload, multiplies by 0.2.

### Example 4: Polynomial Eval Expression

CVT oil temperature conversion from a lookup-like polynomial:

```xml
<parameter id="CVTTemp">
  <description lang="ru">
    <name>Темп. CVT</name>
    <unit>celsius</unit>
  </description>
  <base-data>Core03</base-data>
  <eval>(0.000000002344*(get(13)^5))+(-0.000001387*(get(13)^4))+(0.0003193*(get(13)^3))+(-0.03501*(get(13)^2))+(2.302*get(13))+(-36.6)</eval>
</parameter>
```

### Example 5: Raw Command with Text Response

Reading the Vehicle Identification Number:

```xml
<parameter id="VIN">
  <description>
    <name>VIN Code</name>
    <description>Vehicle's VIN code</description>
  </description>
  <mode>9</mode>
  <command>2</command>
  <text/>
</parameter>
```

### Example 6: Hex Dump Sensor

Displaying raw tire sensor IDs:

```xml
<parameter id="ID_TireSensor_1">
  <base-data>TireSensorID</base-data>
  <dump offset="1" length="4"/>
</parameter>
```

### Example 7: KWP2000 Raw Command with Text

```xml
<parameter id="VIN">
  <raw>1A90</raw>
  <text/>
</parameter>
```

### Example 8: Signed Word Value with Little-Endian

```xml
<parameter id="SteeringAngle">
  <base-data>CoreSensor</base-data>
  <wordle-8>0.0018252</wordle-8>
  <signed/>
</parameter>
```

### Example 9: Class-Based MIL Sensor

```xml
<parameter id="MIL">
  <class>KWPMILSensor</class>
  <raw>1800FF00</raw>
  <mil-group>HyundaiSRS</mil-group>
</parameter>
```

### Example 10: Average Sensor

```xml
<parameter id="Avg_ECUFuelLevel">
  <description lang="ru">
    <name>Уровень топлива (ЭБУ, сред.)</name>
    <unit>liters</unit>
  </description>
  <base>ECUFuelInTank</base>
  <average length="15"/>
</parameter>
```

### Example 11: File with Includes and Global Configuration

```xml
<?xml version="1.0" encoding="utf-8" ?>
<parameters namespace='Mitsubishi_4N15'
            description="Mitsubishi 4N15+AT_V8AWG+TPMS+OBD">
  <disable-obd2>false</disable-obd2>
  <obd2-header>ATSH7E0; ATCRA7E8; ATFCSH7E0;</obd2-header>
  <header>ATSH7E0; ATCRA7E8; ATFCSH7E0;</header>
  <init-string>
    ATSP6; ATST32; ATAT0; ATSH7E0; ATCRA7E8; ATFCSH7E0; ATFCSD300040; ATFCSM1;
  </init-string>
  <models>Mitsubishi</models>

  <include>mitsubishi_eng_4n15.ecuxml</include>
  <include>mitsubishi_at_v8awg.ecuxml</include>
  <include>mitsubishi_ks_tpms.ecuxml</include>

  <!-- Additional parameters specific to this combination -->
</parameters>
```

---

## File Organization

ECUXML files in the HOBDrive project are organized by vehicle manufacturer:

```
ecu/
├── obd2.ecuxml              # Standard extra OBD2 sensors
├── kwp2000.ecuxml           # Common KWP2000 protocol sensors
├── _cuts.ecuxml             # Patch: filter noisy sensor values
├── _gpsspeed.ecuxml         # Patch: use GPS speed instead of ECU
├── _micas_obd2.ecuxml       # Patch: fuel trim for Micas ECUs
├── _vaz_obd2.ecuxml         # Patch: fuel trim for VAZ vehicles
├── _toyotajdmclear.ecuxml   # Patch: JDM MIL clear logic
├── China/                   # Chinese vehicles (Chery, Geely, Haval, etc.)
├── ChryslerDodgeJeep/
├── CitroenPeugeot/
├── FiatLanciaAlfaRomeo/
├── FordMazda/
├── GM/
├── HyundaiKIA/
├── Mercedes/
├── Mitsubishi/
├── Nissan/
├── Opel/
├── Renault/
├── SsangYong/
├── Subaru/
├── Suzuki/
├── Toyota/
├── VAG/
└── VAZ_GAZ_UAZ_ZAZ/
```

Files with an underscore prefix (`_*.ecuxml`) are "patch" files that modify or override existing sensors.

---

## Build Pipeline

ECUXML files are processed by the `ecuxml2cs` build tool:

1. **Parse:** All `.ecuxml` files are parsed by `ECUXMLParser` into `ECUXMLRootElement` objects.
2. **Generate sensor providers:** Each ECUXML file generates a C# class (via T4 template `EcuXmlTemplate.tt`) that extends `BaseECUXMLSensorProvider`. The generated class contains pre-parsed parameter definitions as `ECUXMLParameterElement` objects.
3. **Generate registry:** `ECUXMLSensorProviderRegistry.cs` — maps `.ecuxml` filenames to their generated sensor provider classes.
4. **Generate description registry:** `ECUXMLDescriptionRegistry.cs` — stores ECU profile metadata (name, init string, models) for profiles that have `<models>` defined.
5. **Generate ECU list:** A markdown file listing all available ECU profiles.

At runtime, `ECUXMLSensorProvider` first checks the compiled registry for a matching filename; if not found, it falls back to parsing the ECUXML file directly from disk via `FilesECUXMLSensorProvider`.

---

## Quick Reference: All Parameter Child Elements

| Element | Description |
|---------|-------------|
| `<class>` | C# class name implementing this sensor |
| `<mode>` | OBD2 mode (hex), default: `1` |
| `<command>` | OBD2 PID (hex) |
| `<address><byte>0xNN</byte></address>` | Shorthand for mode 1 + PID NN |
| `<raw>` | Raw hex command string (semicolons → CR) |
| `<header>` | Per-sensor ELM327 header override |
| `<data-offset>` | Reply command length override |
| `<base>` | Reference to base sensor (derived) |
| `<base2>` | Reference to second base sensor (derived) |
| `<base-raw>` | Reference to base sensor (raw byte access) |
| `<base-data>` | Reference to base sensor (data-offset byte access) |
| `<value>` / `<valuea>` | Scale factor, byte at offset 0 |
| `<valueb>` | Scale factor, byte at offset 1 |
| `<valuec>` | Scale factor, byte at offset 2 |
| `<valued>` | Scale factor, byte at offset 3 |
| `<value-N>` | Scale factor, byte at offset N |
| `<valueab>` | Scale factor, big-endian word at offset 0 |
| `<valuebc>` | Scale factor, big-endian word at offset 1 |
| `<valuecd>` | Scale factor, big-endian word at offset 2 |
| `<word-N>` | Scale factor, big-endian word at offset N |
| `<wordle-N>` | Scale factor, little-endian word at offset N |
| `<dword-N>` | Scale factor, big-endian dword at offset N |
| `<dwordle-N>` | Scale factor, little-endian dword at offset N |
| `<signed/>` | Interpret value as signed integer |
| `<offset>` | Additive offset applied after scaling |
| `<bit>` | Right-shift amount for bit extraction |
| `<bit-width>` | Number of bits to extract (default: 1) |
| `<cut-low>` | Zero result if below threshold |
| `<cut-high>` | Zero result if above threshold |
| `<eval>` | Dynamic expression (overrides scale/offset) |
| `<text/>` | Mark as text sensor |
| `<dump/>` | Mark as hex dump sensor |
| `<average/>` | Mark as averaging sensor |
| `<mil-group>` | MIL/DTC group identifier |
| `<description>` | Localized name, description, unit |
