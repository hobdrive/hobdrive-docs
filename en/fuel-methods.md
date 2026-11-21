# Fuel calculation methods and calibration

hobDrive can calculate fuel from MAF, MAP, injector pulse width, cycle consumption, engine load, or a fuel-rate sensor supplied by the ECU. The available data determine which method is usable. Select the method in vehicle fuel settings; calibrating a factor does not automatically choose a different method.

## Methods and parameters

| Method | Inputs and adjustable parameter |
|---|---|
| MAF | Air mass flow and air-to-fuel mass ratio (AFR); default AFR is 14.7. |
| MAP | Manifold pressure, intake temperature, RPM, displacement and volumetric efficiency (VE). VE defaults to 95%; when displacement is specified without VE, hobDrive adjusts this default by displacement / 1.8, capped at 99%. |
| Injector | Injector pulse width, RPM, cylinder count and injector flow. The historical default is 134.23 ml/min, representing half the physical injector flow. |
| Engine load | A global load factor and an RPM-dependent curve. The default global factor may depend on displacement. This approximate method requires verification. |
| Cycle consumption | Cycle fuel quantity, RPM, cylinder count and a density/conversion coefficient. The calculation divides by this coefficient. |
| Built-in ECU fuel rate | The ECU's fuel-rate value. Calibration now supplies a separate multiplier, default 1, for each fuel. It does not alter the source ECU sensor. |

The quick correction screen uses the effective current parameter, including defaults. Changing the model's AFR or VE does not tune the ECU's mixture or engine control. Do not change the real displacement or cylinder count just to make fuel totals agree.

## Quick fuel correction

Open **Actions → Fuel consumption calibration → Calibrate fuel**. The screen uses the currently saved method and selected fuel. Save any method change in vehicle settings before calibrating it.

### By average consumption

This is the default mode. Choose the refuelling average from the latest full-tank interval, the latest three, 90 days, or all completed intervals. hobDrive offers its existing week, month and all-time averages, with the latest measurement date and recorded distance. An available monthly average is preferred, then weekly or all-time. These are the existing period counters, not rolling 7-day or 30-day windows.

Either side can use **Enter my own value**. Missing data offers another period or manual entry. Period boundaries need not match; the proposal is labelled approximate.

Example: refuelling consumption of 8.4 l/100 km versus hobDrive at 8.0 proposes 5% more calculated fuel. MAF AFR 14.7 becomes 14.0. The calculation automatically handles the inverse relationship for mpg and km/l.

Review and select **Apply correction**. Existing averages change gradually with new trips. Averages containing data from before a known fuel or distance correction remain visible but cannot automatically trigger another correction. Choose a later period, or enter a new measurement made with current settings and note its dates. Reapplying identical entered evidence is also blocked.

On dual-fuel vehicles, the comparison uses the selected fuel over the whole vehicle distance, matching refuelling records. This can differ from consumption over distance driven only on that fuel. Manually entered figures must use the same interpretation.

### By fuel volume

For a comparison over matching sections, select **By fuel volume**. Compare **two fuel volumes over identical intervals**:

- fuel actually consumed between full tanks;
- fuel calculated by hobDrive over exactly those intervals, with the displayed current settings.

Exclude the initial full fill; include every subsequent partial fill and the final full fill. Two arbitrary refuelling records without equivalent known tank levels do not establish consumption.

You can use actual volumes from the selected completed intervals or enter your own measurements. Enter the calculated volume from matching hobDrive statistics. Do not compare a monthly average with a different refuelling period. If matching historical measurements are unavailable, collect a new control interval.

Review the old and proposed parameter and the expected percentage change. Confirm comparability, then select **Apply correction**. Both decimal commas and decimal points are accepted.

## Correction direction

For volume calibration, let `R = actual fuel volume / hobDrive calculated volume`. For average calibration, `R = refuelling consumption / hobDrive consumption`, after converting both values to l/100 km.

| Method | Parameter changed | New value |
|---|---|---|
| MAF | AFR | `AFR / R` |
| MAP | VE | `VE × R` |
| Injector | Injector flow | `flow × R` |
| Engine load | Global load factor | `factor × R` |
| Cycle consumption | Cycle fuel coefficient | `coefficient / R` |
| Built-in ECU rate | ECU fuel-rate correction | `correction × R` |

**AFR and the cycle coefficient are divisors.** Increasing them reduces calculated fuel. MAP calibration adjusts VE, leaving AFR, displacement and cylinder count unchanged. Secondary-fuel calibration changes only that fuel's parameter.

Example: the full-tank records show 60 litres and hobDrive calculated 55 litres. `R = 60 / 55 = 1.090909`, requiring about 9.09% more calculated fuel. With MAF AFR 14.7, the proposed AFR is `14.7 × 55 / 60 = 13.475`. Multiplying AFR by `60 / 55` would incorrectly reduce fuel further.

## Separate fuel from distance

The same 60 litres over 600 and 630 km give 10 and 9.52 l/100 km. This alone does not prove a fuel-calculation error. Average calibration can also compensate for a distance difference to align the displayed figures with the chosen reference. Check fuel calibration again after correcting distance. To examine fuel separately, compare volumes over identical sections.

See [speed, distance and reference choice](speed-diff.md). Independent GPS distance currently comes from a verified external measurement; a separate automatic GPS-distance accumulator is not available for calibration yet.

## Measurement quality and history

A coefficient cannot repair missing trips, missing receipts, manual fuel/distance additions, incorrect units or a change of method halfway through the interval. Compare each fuel separately on dual-fuel vehicles.

In volume mode, confirm that hobDrive recorded the full intervals and the displayed current settings applied throughout. If this is unknown, start a new control interval. Agreement on a single tank does not prove accuracy under every operating condition; validate the correction on subsequent full intervals.

The new factor affects subsequent measurements. Existing totals and historical averages are not recalculated. An interval spanning a coefficient change is not a homogeneous basis for the next calibration. The profile stores the date, input values and settings before each application; the screen displays the five most recent corrections. This is a history of calibration actions, not a complete history of all vehicle settings.

Warm idle fuel rate can expose gross method errors, but a rule of thumb such as half the engine displacement is not a measured calibration reference.
