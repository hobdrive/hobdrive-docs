# Visuals, Screens, Layout, and DashKits

HobDrive lets you customize the main interface on several levels: from simple switches in the app to custom XML screens and full DashKit packages with graphics, themes, and multiple panels.

This article is an overview. It helps you choose the right customization method and understand where the detailed instructions are. The full tag and attribute reference is available in the [HobDrive layout language specification](LAYOUT_SPEC.md).

## What Can Be Customized

Depending on the task, you can change:

- the order of main screens;
- which screens are available by swiping and which open only through the **Screens** menu;
- personal screen options, if they are provided by the layout;
- the sensor set on the custom **Your Screen** screen;
- screen XML layout through `user.layout`;
- sensor appearance through `user.gauge` and `item` attributes;
- screen sets, images, and themes through DashKits.

If you are configuring HobDrive for the first time, start with the interface settings. XML and DashKits are needed when the regular settings are no longer enough.

## Customization Levels

### 1. Main Screen Settings

Open **Screens -> Settings -> Main Screen Settings**.

This dialog lets you:

- pin screens to the main panel with **Always Show**;
- remove rarely used screens from swiping while leaving them in the **Screens** menu;
- change screen order through **Screen Position**;
- enable personal options for some screens;
- build one custom **Your Screen** screen without manually editing XML.

Details: [Main Screen Settings](screen-settings.md).

### 2. Custom Screen `user1.layout`

The **Your Screen** section is saved to the `user1.layout` file. Usually you do not need to edit it manually: the app creates XML itself when you choose the layout, sensors, and additional cell parameters.

Use **Export XML** and **Import XML** to save, transfer, or quickly share a custom screen.

`user1.layout` works well for one personal screen, for example "city", "highway", "diagnostics", "temperatures", or "hybrid".

### 3. Manual `user.layout`

The `user.layout` file is the advanced way to add your own sections and change existing screens. It is loaded from user settings and should not be overwritten by app updates.

Through `user.layout` you can:

- add a custom screen with `<section>`;
- describe a sensor grid with `<grid>`;
- show a screen only when required sensors are present through `if`;
- add buttons and actions;
- use nested grids, `switch`, `union`, decorators, and images;
- build screens for a specific ECU, profile, or device orientation.

Minimal example:

```xml
<ui>
  <section name="My Screen" fixed="true">
    <grid rows="25,,30" cols="30,,30">
      <item id="FuelLevel" precision="1"/>
      <item id="FuelEconomy_trip" size="large"/>
      <item id="BatteryVoltage" size="medium" period="5000"/>
      <item id="RPM"/>
      <item id="Speed" size="giant"/>
      <item id="CoolantTemp"/>
      <item id="IntakeAirTemp"/>
      <item id="DistanceRun"/>
      <item id="DateTime" custom-units="timesec" units="none"/>
    </grid>
  </section>
</ui>
```

Syntax details: [Layout specification](LAYOUT_SPEC.md).

### 4. `user.gauge` and Sensor Appearance

`layout` describes which elements are on the screen and where they are located. `gauge` settings describe how those elements look: sizes, colors, borders, charts, decorators, images, and inherited styles.

Important: `user.gauge` may be changed by HobDrive itself. The app writes settings there from the sensor appearance dialog opened by long-pressing a sensor. For this reason, manual `user.gauge` editing is possible, but should be done carefully: interface changes and manual edits should not conflict with each other.

In practice, this combination is common:

- in `user.layout`, an element gets `inherit="_SomeStyle"`;
- in `user.gauge` or a local `gauge`, a set of visual attributes is defined;
- a specific `item` overrides individual attributes when needed.

For simple changes, attributes directly on `item` are usually enough: `size`, `precision`, `period`, `units`, `description`, `inherit`, `colspan`, `rowspan`.

More about configuring sensor appearance through the interface: [Main Screen Settings](screen-settings.md#sensor-appearance-settings).

### 5. DashKits

A DashKit is a package with ready-made panels, screens, images, and metadata. DashKits let you distribute not just one settings file, but a whole visual package: for example, a digital panel, analog dashboard, theme, or diagnostic screen set.

Official repository: [hobdrive/hobdrive-dashkits](https://github.com/hobdrive/hobdrive-dashkits).

Template for creating your own repository: [hobdrive/template-dashkits](https://github.com/hobdrive/template-dashkits).

In the app, DashKits are opened through **Settings -> Dash Panel Settings**. The dialog contains:

- **Installed dash panels** - already downloaded packages;
- **Available dash panels** - packages from the selected GitHub repository;
- **Source settings** - `Repo` and `Branch` fields, for example `hobdrive/hobdrive-dashkits` and `main`;
- actions **Install**, **Update**, **Delete**, **Preview**.

By default HobDrive looks at `hobdrive/hobdrive-dashkits`, branch `main`. You can specify your own repository in `owner/repo` format if you are building custom panels.

## How a DashKit Is Structured

A typical DashKit lives in its own folder and contains:

```text
my-dashkit/
  info.json
  user.layout
  images/
  README.md
```

`info.json` describes the package:

```json
{
  "name": "My DashKit",
  "version": "1.0.0",
  "description": "Custom dashboard for HobDrive",
  "author": "Your Name"
}
```

`user.layout` contains ordinary HobDrive XML layout. `images/` stores pictures, SVGs, and other resources referenced by layout elements.

When installing, HobDrive downloads the DashKit folder from GitHub and places it in user settings as a separate `override-*` directory. This lets several packages coexist, update, and be removed independently.

## What to Choose

- Need only to change screen order and visibility: use [Main Screen Settings](screen-settings.md).
- Need to build one screen from the sensors you need: use **Your Screen**.
- Need to add a couple of custom XML screens: use `user.layout`.
- Need to create a full panel with graphics, a theme, several screens, and distribution through GitHub: make a DashKit.
- Need to understand the exact tag and attribute syntax: go to [LAYOUT_SPEC.md](LAYOUT_SPEC.md).

## Practical Tips

- Do not edit standard `default-*.layout` and `default.gauge` files: app updates may replace them.
- Keep your changes in `user.layout`, `user.gauge`, `user1.layout`, or a DashKit.
- Before experimenting, make a settings backup: [How to transfer settings or make a backup](backups.md).
- Use `if` conditions so a screen appears only when the required sensors are available.
- For dense diagnostic screens, check readability on a real device.
- Test complex interface changes while parked, not while driving.
