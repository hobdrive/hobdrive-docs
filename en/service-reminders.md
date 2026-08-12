[English](service-reminders.md) | [Русский](../ru/service-reminders.md)

# Service Intervals and Reminders

The **Service reminders** screen helps you schedule maintenance by mileage, time, or both. Each plan belongs to the current vehicle profile and is stored separately from the journal of completed work.

The bundled intervals are starting points, not a manufacturer-approved schedule for a specific vehicle. Check every interval against the owner's manual or your service provider's recommendations before saving it.

## Opening the Screen

Open HobDrive's main screen list and select **Service reminders**. Tapping a due-service notification on Android or iOS also opens this screen and highlights the relevant reminder.

The top of the page shows the current vehicle profile and odometer. If the wrong vehicle is selected, switch profiles first: plans and statuses are calculated separately for each profile.

## First Setup from a Preset

1. Tap **Setup**.
2. On **Setup service intervals**, select a group:
   - **Scheduled service** — one general recurring service visit;
   - **Owner maintenance** — oil, filters, and other consumables;
   - **Safety** — brakes, brake fluid, and wheels;
   - **Drivetrain** — transmission oil, coolant, and belts;
   - **Seasonal** — tires, battery, and air conditioning.
3. On **Review intervals**, leave only the rows you want selected.
4. Turn off **Recommended only** if you want to see the additional intervals in that group.
5. Review the repeat intervals, advance warnings, and last-service starting values.
6. Tap **Save**.

Plans that already exist are marked **Already added** and are not created again.

## Interval Fields

- **Service** — the type of work, such as engine oil or brake-fluid replacement.
- **Category** — the category used for maintenance and expense records.
- **Repeat km** — how many kilometres to travel before repeating the work.
- **Repeat days** — how many days to wait before repeating the work.
- **Advance km** and **Advance days** — when the reminder changes to **Soon**.
- **Last date** and **Last odometer** — the starting point for the next due calculation.
- **Custom title** — an optional user-defined name.

At least one repeat interval, kilometres or days, is required. If both are set, HobDrive evaluates both rules and displays the more urgent status. For example, the annual limit can become due before the mileage limit.

Enter the actual last-service date and odometer carefully. Without a usable starting point, the plan may appear as **Needs setup**.

## Adding a Custom Reminder

Tap **Add custom reminder** on the main page or inside the setup dialog. Select the service and category, optionally enter a custom title, and then set:

- a mileage and/or time interval;
- the advance-warning distance or time;
- the last-completed date and/or odometer.

Custom reminders are useful for non-standard work, accessories, or an interval shorter than the bundled preset.

## Statuses and Tabs

The dashboard sorts plans into five tabs:

- **Attention** — shows **Overdue** and **Due** in separate groups;
- **Soon** — the configured advance-warning threshold has been reached;
- **Scheduled** — active plans that are not approaching their due point, plus **Needs setup** plans;
- **Paused** — snoozed and disabled plans;
- **All** — every plan grouped by status.

Each card shows the service name, status, remaining or overdue time/distance, last completion, and next due point.

## Acting on a Reminder

### The Work Is Complete

Tap **Done**. HobDrive opens a maintenance-record form with the service selected, today's date, and the current odometer when available. Review the values, add the cost and notes, and save the record.

After saving:

- the work appears in the normal event journal;
- its date and odometer become the plan's new starting point;
- the next due point is calculated automatically;
- the previous notification is cleared.

Closing the form without saving leaves the reminder unchanged.

### Postpone the Work Briefly

Tap **Snooze**. The current version snoozes for 2 days and, when an odometer is available, 100 km. The reminder returns when either boundary is reached. Snoozing does not create a completed-service record.

### Skip This Cycle

Tap **Skip** and confirm. HobDrive records the skipped cycle in the journal and advances the plan without marking the maintenance as completed. Use **Snooze** for a short delay; use **Skip** only when you genuinely do not intend to perform the current cycle.

### Edit or Manage the Plan

Tap **Edit** to change the service, title, category, repeat intervals, advance warnings, and last-service starting values.

The **Manage** menu contains:

- **Disable** — keeps the plan but stops its calculation and notifications; use **Enable** on the **Paused** tab to resume it;
- **Delete** — removes only the reminder plan. Existing maintenance records remain in the journal.

## Notifications

HobDrive sends local system notifications for **Due** and **Overdue** statuses. **Soon** is visible on the dashboard but does not send a notification by itself.

Date-based reminders can be scheduled in advance. A mileage-only reminder is recalculated when HobDrive receives an up-to-date odometer, so open the app periodically and keep its mileage accurate. HobDrive must also be allowed to show notifications in Android or iOS system settings.

## If a Status Looks Wrong

- Confirm that the correct vehicle profile is selected.
- Open **Edit** and verify the last date and odometer.
- Make sure at least one repeat interval is set.
- Mileage calculation requires a current odometer; without it, only a date interval can continue working.
- After real maintenance, use **Done**, not **Skip**, so the journal keeps the full record, cost, and notes.

Service plans and the maintenance journal are part of HobDrive user data and are included in backups. See [Data Management, Backups, and Cloud](user-manual.md#data-management-backups-and-cloud).
