# Main Screen Settings

In HobDrive you can configure which screens are available on the main screen panel, their order, which ones are always visible while swiping, and which ones remain available only in the **Screens** menu. Some screens have their own display options, and the **Your Screen** section lets you build one custom screen from selected sensors.

This article describes configuration through the app interface. If you need to change the XML screen layout manually, see the technical articles [Layouts, Screens, and DashKits](layouts.md) and [Layout Format Specification](LAYOUT_SPEC.md).

## How to Open Screen Settings

Open **Screens -> Settings -> Main Screen Settings**.

The section list is on the left side of the dialog:

- **General Screen Settings** - options that affect several screens at once.
- Regular HobDrive screens - for example, the main screen, trip computer, fuel details, GPS, diagnostics, and other screens available in your configuration.
- **Your Screen** - a custom screen you can build from selected sensors.

On a phone or narrow screen, the list may open through the **≡** menu button. The **‹** and **›** buttons switch to the settings for neighboring screens.

## What "Always Show" Means

The **Always Show** switch pins the screen to the main screen panel.

If this option is enabled:

- the screen is available through normal swiping;
- the screen is visible in quick navigation;
- the screen remains part of the main set even after the interface is reloaded.

If this option is disabled:

- the screen does not take space in the main sequence;
- it can still be opened through the **Screens** menu if it is available for the current vehicle configuration;
- this is convenient for rarely used screens, such as diagnostic, service, or special ECU screens.

This lets you keep only the screens needed during a drive in front of you, while removing the rest from everyday swiping.

## How to Change Screen Order

The **Screen Position** option sets the screen priority in the overall sequence.

The **Default** value means the default order defined by the current HobDrive layout. Numeric values let you move a screen relative to the standard order:

- negative values place the screen before default screens;
- positive values place the screen after unnumbered screens;
- `0` matches the default behavior and is shown in the interface as **Default**.

Practical examples:

- To move an important screen closer to the beginning, enable **Always Show** and set a negative position, for example `-1`.
- To keep a screen available by swiping but closer to the end, set a positive position, for example `1` or `2`.
- If the order becomes inconvenient, return **Screen Position** to **Default**.

After you leave the dialog, HobDrive saves the changes and reloads the screen interface.

## Personal Screen Options

Some screens show an additional block of switches in their settings. These options are defined by the screen layout itself and affect only that screen or screen group.

Examples of such options:

- show date and time on the top panel;
- enable outlines around sensor values or labels for better readability;
- show fuel level from the ECU instead of the estimated level;
- adapt the display for a diesel engine;
- show the gear number with Arabic numerals instead of Roman numerals;
- rotate temperature sensors on the main screen;
- enable the gear shift assistant;
- show average speed excluding idle time;
- show odometer data from the ECU.

The available options may differ depending on the app version, platform, vehicle profile, and available screens. If a specific screen has no additional switches, then only the general visibility and order settings are currently available for it.

## Sensor Appearance Settings

In addition to configuring screens, HobDrive lets you change the appearance of individual sensors. This dialog opens from a sensor screen:

1. On the main screen, long-press the desired sensor.
2. The sensor information window opens.
3. Select the widget appearance settings action.

In the dialog you can change the display type and visual parameters. Depending on the item, options such as `Text`, `Bar`, `RoundBar`, `Chart` may be available, along with related attributes: size, precision, borders, charts, colors, and other display parameters.

The most important option in this dialog is **Applicability Level**. It determines where the change will be written:

- **Global** - the setting applies to all sensor types as the default value.
- **This Sensor** - the setting applies to the current sensor on every screen where it appears.
- **This Widget** - the setting applies only to the current widget: this specific sensor in this exact position on this screen.

Example: if you change the speed size at the **This Sensor** level, speed changes on all screens. If you choose **This Widget**, only the speed cell where you opened the dialog changes.

These settings are saved in `user.gauge`. The file can be edited manually, but carefully: HobDrive also changes it from the interface, so manual edits may be overwritten or mixed with settings made through the dialog.

## Custom Screen "Your Screen"

The **Your Screen** section is intended for quickly creating your own screen without manually editing files.

On this screen you can configure:

- **Always Show** - pin the custom screen to the main panel or leave it in the **Screens** menu.
- **Screen Position** - move it closer to the beginning or the end.
- **Screen Title** - the name shown in the screen list.
- **Choose Layout** - the sensor grid size.
- Cell list - which sensor is shown in each cell.

Available layouts:

`1x1`, `2x2`, `2x3`, `3x2`, `3x3`, `4x2`, `2x4`, `4x3`, `3x4`, `5x3`, `5x4`.

For example:

- `2x2` - a compact screen with four large sensors;
- `3x2` or `2x3` - a convenient option for everyday driving;
- `5x3` or `5x4` - a dense diagnostic screen with many values.

For each cell, press the sensor selector and choose the required sensor. An empty cell remains blank. Usually it is better to start with a small layout, check readability in the car, and then increase the number of sensors if needed.

## Additional XML for a Cell

Each cell on the custom screen has an advanced **Edit XML / Preview** mode. Use it when ordinary sensor selection is not enough.

In the **Additional XML** field you can add:

- additional item attributes, for example size, precision, or update period;
- nested XML elements, if a more complex layout is needed.

Examples:

```xml
size="large" precision="1"
```

```xml
period="5000" units="below"
```

If the text starts with `<`, HobDrive treats it as nested XML inside the `item` element. If the text does not start with `<`, it is added as attributes to the current `item`.

Preview helps you immediately see how the cell will look. If the XML contains an error, fix it before saving.

## XML Import and Export

The **Export XML** and **Import XML** buttons let you save or transfer a custom screen.

**Export XML** shows the full XML of the current custom screen. It is convenient to copy it for backup or send it to another user.

**Import XML** lets you paste ready XML and apply it to the custom screen. If the XML is invalid, HobDrive shows an error and does not apply the layout.

The custom screen is saved in the `user1.layout` file in HobDrive user settings. This file can be included in a normal settings backup. More about data transfer: [How do I transfer all settings or make a backup?](backups.md).

## How This Differs from `user.layout`

The **Your Screen** dialog creates and updates one custom screen in the `user1.layout` file. It is a fast and safe way to build your own sensor set through the interface.

The `user.layout` file remains the advanced configuration path. You can use it to manually add custom sections, complex grids, `if` conditions, decorators, images, buttons, and other layout elements. This approach is more powerful but requires careful XML editing.

Recommended order:

1. First configure screens through **Main Screen Settings**.
2. Use **Your Screen** for your own sensor set.
3. If the dialog is not enough, move to manual `user.layout` configuration.

## Tips

- Do not overload the main screen set. A few pinned screens are usually enough for a drive.
- Diagnostic and rarely used screens are better left unpinned and opened through the **Screens** menu.
- For an important custom screen, set a clear title and a negative position.
- Check readability on a real device: an overly dense grid may be inconvenient on the road.
- Make complex changes while parked, not while driving.
