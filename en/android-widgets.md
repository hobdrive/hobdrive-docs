[English](android-widgets.md) | [Русский](../ru/android-widgets.md)

# HobDrive Widgets for Android

The HobDrive Android widget displays a selected sensor on the home screen. It uses the same rendering system as the app, can be resized, and keeps separate settings for every widget instance.

This feature is available on Android only. System command names vary slightly between phone, head-unit, and launcher vendors.

## Adding a Widget

1. Touch and hold an empty area of the Android home screen.
2. Open the system **Widgets** picker.
3. Find **hobDrive Sensor Widget** and drag it to the home screen.
4. Android opens **Configure Widget**.

If the widget does not appear immediately after a HobDrive update, restart the launcher or device and check the picker again.

## The Configure Widget Screen

### Select Sensor

Tap **No sensor selected** under **Select Sensor**. In the list that opens:

- enter part of the sensor's user-facing name in the search field;
- enable full-ID search if you know the sensor's internal identifier;
- select the required row.

The button then shows the sensor name, and the **Preview** area displays its appearance. **Save Widget** remains disabled until a sensor is selected.

### Update Frequency

The **Update Frequency** list contains:

- **5 seconds**;
- **15 seconds**;
- **30 seconds** — the default;
- **60 seconds**;
- **On demand (tap to refresh)**.

A shorter interval provides fresher readings but uses more resources. For values that do not need second-by-second updates, choose 30–60 seconds or on-demand refresh.

### Preview and Size

The preview shows the widget styling before you save it. After adding the widget, resize it in the normal Android way: touch and hold it, then drag the resize handles. HobDrive redraws the content for the new width and height.

## Optional Custom XML

**Custom XML (optional)** is intended for experienced users. It replaces the standard sensor presentation with a custom HobDrive layout. For example:

```xml
<item id="Speed" size="large"/>
```

HobDrive validates the XML and refreshes the preview as you edit it. Leave the field empty to use the standard presentation for the selected sensor. If the editor reports an error, correct the XML before saving.

See [Layout Format Specification](LAYOUT_SPEC.md) for available elements and attributes.

## Saving and Using the Widget

Tap **Save Widget** to finish, or **Cancel** to discard the new widget.

Tap behavior depends on the selected frequency:

- in **On demand** mode, tapping refreshes the reading;
- with a periodic frequency, tapping opens the main HobDrive app.

You can add multiple widgets and give each one its own sensor, frequency, size, and XML.

## Reconfiguring an Existing Widget

HobDrive supports widget reconfiguration. Usually, touch and hold the widget and choose the launcher's **Configure**, pencil, or widget-settings command. If your launcher provides no such command, remove the widget from the home screen and add it again.

Removing a widget deletes only that widget's settings; it does not delete HobDrive data.

## If the Widget Has No Data

- Launch HobDrive at least once after installing or updating it.
- Check the current vehicle profile and OBD-adapter connection.
- The selected sensor must be available in the current vehicle configuration. Some values appear only while the engine is running or the connection is active.
- The widget does not wake a sleeping device solely to refresh. It updates after the device wakes or HobDrive receives new data.
- If on-demand mode is selected, tap the widget.
- If the problem continues, reopen **Configure Widget**, select the sensor again, and check the preview.
