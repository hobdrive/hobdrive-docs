# Speed, distance and choosing a calibration reference

The dashboard speedometer, ECU speed, vehicle odometer and GPS measurement do not have to agree. First choose your goal: matching the vehicle or matching independently measured distance.

## Why readings differ

A speedometer may deliberately read **higher**, not lower, than true speed. UN R39, for example, requires indicated speed not to fall below true speed under its prescribed test conditions. This concerns the speedometer; it does not establish an identical error in the odometer or OBD speed. [UN R39, section 5.4](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX%3A42025X1902).

In a typical configuration hobDrive accumulates distance from ECU speed, with the configured speed and distance corrections. A particular profile can use another source, including GPS. In that case, hobDrive and GPS are not independent measurements.

Tyre size, effective rolling circumference and the vehicle's calculations can affect readings. A percentage difference alone does not identify the cause. Speed error does not prove the same odometer error.

GPS is a useful reference, not an absolute ground truth. Accuracy depends on signal reception, obstructions, satellite geometry and receiver quality. Check track quality and gaps. Compare identical sections, not a short GPS fragment with an entire refuelling interval. [GPS accuracy](https://www.gps.gov/gps-accuracy).

hobDrive does not yet accumulate separate historical GPS distance for automatic calibration. The correction form accepts a distance from a GPS track you have checked or another independent measurement.

## Choose the result you want

| Goal | Comparison | Expected outcome |
|---|---|---|
| Match the vehicle | Vehicle odometer increment versus hobDrive distance over the same section | Closer agreement with the car and refuelling journal; not proof of true distance. |
| Match independent distance | Verified independent measurement versus hobDrive over the same section | Closer agreement with that reference. The vehicle odometer and fuel economy calculated from it may still differ. |

Accepting hobDrive's suggested odometer when entering a refuelling record is not an independent vehicle reading. For the first option, record the actual dashboard odometer at both boundaries.

## Quick distance correction

Open **Actions → Fuel consumption calibration → Calibrate distance**.

1. Choose the vehicle odometer or independent distance as your reference.
2. Enter distance travelled, not an absolute odometer reading: subtract the start from the end. For the vehicle reference, selected completed refuelling intervals can supply the odometer differences.
3. Separately enter hobDrive's distance for the same sections. When using refuelling records, compare only the sum of the selected intervals, excluding any gaps between them.
4. Review the proposed factor and confirm that the measurements are comparable.
5. Select **Apply correction**.

Formula: `new distance factor = current factor × reference distance / hobDrive distance`.

Example: factor 1, reference 600 km, hobDrive 570 km gives `1 × 600 / 570 = 1.052632`.

This changes the **odometer/distance correction**. The separate **speed correction** stays unchanged. Changing speed correction, tyres or speed source later requires a new distance comparison.

The factor affects subsequent distance increments. It does not set the absolute odometer or rewrite accumulated history. Synchronising the current odometer during refuelling is a separate operation.

## A factor cannot recover missing trips

Without the required telemetry, hobDrive cannot reliably calculate distance or fuel. Synchronising the odometer at refuelling does not reconstruct missing measurements. Use fully recorded sections without manual additions or changes of settings.

If you do not know which factors or method applied to older data, collect a new comparable section. The calibration screen stores the date and basis of each correction it applies; changes made in other settings do not yet have a complete history.

## The effect on refuelling fuel economy

The same 60 litres over 600 km give 10 l/100 km; over 630 km they give 9.52 l/100 km. A 5% distance difference produces about 4.8% difference in l/100 km without any difference in fuel volume.

Choosing independent distance therefore need not make hobDrive fuel economy match refuelling fuel economy based on the car's odometer. Quick calibration by average consumption may compensate for distance differences too; check it again after adjusting distance. To examine fuel separately, compare volumes over identical intervals. See [fuel methods and calibration](fuel-methods.md).
