[English](statistics.md) | [Русский](../ru/statistics.md)

# Trip Statistics and Online Analytics

HobDrive records trips, fuelings, expenses, and selected vehicle readings. You can view these in a local in-app report or synchronize them to the cloud and open **Hobdrive Analytics** from a phone or computer.

Cloud analytics is an optional service. Availability, permitted synchronization frequency, and subscription term depend on your license.

## Data Included in Reports

Report detail depends on what the app records:

- trips with their date, duration, distance, average speed, and fuel economy;
- the GPS track when location logging is enabled;
- selected OBD and calculated sensors;
- fuelings with amount, cost, unit price, odometer, and notes;
- events and expenses such as maintenance, parts, consumables, wheels, accessories, taxes, and custom categories.

For meaningful expense analytics, consistently enter the odometer, cost, currency, and category. Full-tank entries in an uninterrupted sequence provide the most useful fueling-based economy figures.

## Configuring Trip and Sensor Logging

Open **Settings → Data Logging**. This screen includes:

- **Location Logging** and its frequency;
- **Speed Logging**;
- **Background Sensor Logging**;
- **Sensor Readings Logging** for selecting additional sensors and their frequency;
- logging duration and gap controls for diagnostic sessions.

More sensors and shorter frequencies produce more data. GPS, speed, and the main trip values are normally sufficient for routine statistics; use high-frequency extra-sensor logging for a specific diagnostic purpose.

## Local In-App Statistics

Open the main **Statistics & Events** screen. HobDrive prepares a report with trips, fuelings, and records grouped by period. In older app versions, the same workflow may appear as **Actions → Update statistics** followed by **Open statistics**.

The report lets you move between all-time, year, month, and day pages, open individual trips, and view maps and charts. The **Fuelings** and **Events** pages support sorting, searching, and filtering.

The local report does not require a cloud subscription. Features that require server processing or a broader history may be available only in the online report.

## Enabling Cloud Analytics

1. Open **Settings → Data and Backups**.
2. Enable **Store Data in Cloud** and accept the data-transfer notice.
3. Under **What data to upload to the cloud?**, select the required scope. For a complete report with routes and comparison analytics, use **All Data (Settings, Sensor Data, Location)**. **Settings Only** does not upload trip history for analytics.
4. If you have a separate cloud license, enter its **License ID**.
5. Select the **Cloud upload interval**. The app will not allow a frequency higher than the current license permits.
6. Tap **Check Subscription** and confirm that it is active.
7. Tap **Sync Now** for the initial upload and wait for **Synchronized**.
8. Tap **Open Hobdrive Analytics**.

Uploaded data can include location and vehicle parameters. Choose the upload scope deliberately. Contact HobDrive support if you need all server-side data to be deleted.

## Navigating the Online Report

The top menu contains the main report areas:

- **All time**, **Years**, and **Months** for summaries and trip lists;
- **Fuelings** for the fueling journal and fuel analytics;
- **Events** for maintenance and other expenses.

From a period page, you can move to a shorter period, open an individual trip, or show its route on a map. Tables can be sorted, and some columns use a green-to-red scale to expose the better and worse values within the current list.

## Trip Comparison Analytics

When a comparison is available, a colored **A** marker with a score appears beside the row date. Tap it to expand analytics for that trip or period. **Show Analytics** expands or hides the calculations for every available row at once.

The comparison panel contains:

- **Score** — an overall result relative to the baseline: positive is better, negative is worse, and a value near zero is close to the expected result;
- the **Comparison source** — same route, similar trips, or a typical profile;
- baseline sample count and **confidence**;
- differences in time, fuel, economy, average speed, and cost.

Color helps with interpretation: green is a more favorable result and red is less favorable. For individual metrics, the sign is the actual value minus the baseline. This means a negative time or fuel difference is usually good, while a positive speed difference can be good. Read the color together with the metric name.

Tap the source name to see how the baseline was built. When the sample count is a link, tap it to open the trips used in the comparison.

A day or month summary also shows:

- **Coverage** — how many trips and kilometres were included;
- the baseline sources used;
- aggregate differences for the whole period.

HobDrive first tries to compare a trip with previous journeys on the same route. If there are too few, it uses trips with a similar distance and speed profile, then a typical model. When history or fuel data is insufficient, no comparison marker appears or the baseline is shown as unavailable.

## Fueling and Expense Charts

The **Expense analytics** panel appears on the **Fuelings** and **Events** pages.

### Selecting Records

Use **Records** to switch between:

- **All** — fuelings and other expenses together;
- **Fuelings** — fuel only;
- **Events** — maintenance and other costs.

Use **All time**, **12 months**, **Year**, or exact **From / To** dates. Data can be grouped by day, week, month, quarter, or year. Currency and category filters appear when the data contains more than one value.

HobDrive does not convert currencies. If records contain multiple currencies, select one currency before interpreting totals or charts.

### Summary Values

The KPI cards depend on the selected records:

- Fuelings: total cost, fuel amount, average unit price, and average fuel economy.
- Events: total cost, record count, average expense, and cost per 1,000 km.
- All: total cost, fuelings, other expenses, and cost per 1,000 km.

Cost per 1,000 km requires enough valid odometer readings. Records without a cost are included in the record count but add nothing to the total.

### Charts

- **Expenses over time** shows cost in each selected period. Enable **Cumulative total** to show the running total.
- **Show empty periods** inserts intervals with no records, making gaps on the time axis explicit.
- Tapping a period bar or point filters the table to that period. The active period appears above the table; tap **×** to clear it.
- **Expense breakdown** groups cost by expense type, category, tag, or fuel type. Available breakdowns depend on the **All / Fuelings / Events** mode.

The source table below the charts can be searched, sorted, and paged. Totals recalculate for the current filter. Fuel-economy columns in the fueling journal use a green-to-red scale.

## If New Trips Do Not Appear

- In **Data and Backups**, check **Cloud Status**, the subscription, and any upload-frequency limit.
- Tap **Sync Now** and wait for completion.
- Make sure the upload scope is not **Settings Only**.
- For routes, check location permission and **Location Logging**.
- For comparisons, collect several trips and make sure distance, time, and fuel values are being recorded.
- Confirm that you opened the report for the correct license ID. Contact support if the analytics link does not open.

Cloud synchronization does not replace a backup of settings and user files. See [Data Management, Backups, and Cloud](user-manual.md#data-management-backups-and-cloud).
