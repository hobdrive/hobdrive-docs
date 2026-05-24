- v1.9.4 14/03/2026
  - First attempt at implementing Widgets for Android.
  - Fixed black screen (zero brightness) on some head units.
  - GPS sensors were not displayed (broken in 1.9.3).
  - Refuelings and events were not shown or recorded in statistics (broken in 1.9.3).
  - Reduced brightness of red flashing when connection error occurs.
  - Support for duplicate CarPlay screen on iOS.
  - Various minor UI improvements.
  - Sensor selection screen in settings is now more convenient, with search capability and display of currently selected sensor.
  
- v1.9.2 08/02/2026
  - Added odometer (mileage) correction coefficient. Separate from speed - allows correcting mileage if needed,
    to align it with the built-in odometer readings.
  - Fixed DashKit screen operation.
  - Improvements in other settings screens.
  - In some cases offline license verification was not working - fixed.

- v1.9.1 31/01/2026
  - Fixed crashes when switching screens.
  - Gear indicator can be configured to display Roman or Arabic numerals (screen settings).
  - Added graphs and direction improvements to the GPS screen

- v1.9.0 11/01/2026
  - Accelerated rendering of UI elements (OpenGL renderer enabled by default on all platforms)
  - Conditional element "if" is now supported in gauge descriptions
  - DashB3: uses effects and filters to improve appearance
  - Sensor values are rendered with outlines for better readability
  - Text and description outline rendering is configurable, can be disabled in UI settings.
  - Fixed button display in sensor details dialog

- v1.8.20 04/01/2026
  - Fixed error reset buttons
  - Improvements in data and cloud management screen, settings for cloud upload frequency, deletion of old data

- v1.8.19 02/01/2026
  - Fixed Bluetooth connection.
  - Fixed DashKit skin selection dialog
  
- v1.8.18 01/01/2026
  - Improved UI settings and themes
  - Added ability to select multiple background images in each theme
  - Themes can be added via DashKits. BlueRain2 theme is available as an example.
  - Fixed broken "All Sensors" screen, now you can view all sensor values at once.

- v1.8.17.2 05/12/2025
  - Automatically corrected missing translations in all languages.
  - ECU: 
    Opel VB X20DTH
    Opel AstraJ A14NET+OBD CAN

- v1.8.17 01/12/2025
  - Test build via maui

- v1.8.16 17/10/2025
  - License verification issue from website - fixed.

- v1.8.15 17/10/2025
  - Experimental support for Dash Panels - ready-made skins and screens from the network.
    "Dash Panels Settings" dialog

- v1.8.12 03/10/2025
  - Issue with overlapping error reset buttons
  - Issue on dual-fuel mode - flow meter parameters were always taken from the primary fuel type (broken in v1.8.7)
  - iOS: crashes when working with Bluetooth 4 adapters

- v1.8.11 29/09/2025
  - Fixed calculation of driving efficiency
  - Additional fields in sensor settings, more descriptions.

- v1.8.10 20/09/2025
  - Minor fixes for release

- v1.8.9 16/09/2025
  - Android:
    - More fixes for screen boundaries
    - Fixed crash in settings when clicking on some controls
  - New:
  - You can now set filters for sensors:
    - Smoothing and predicting filter: <item id="Speed" wrap="predict(50, 0.7)"
      50 - prediction frequency in milliseconds, 0.7 - prediction degree
    - Smoothing filter with delay: <item id="Speed" wrap="smooth(400,100,5)"
      400 - delay time in milliseconds
      100 - update interval in milliseconds
      5 - minimum sensor value difference to ignore smoothing
    - Averaging filter: <item id="Speed" wrap="average(30)"
      30 - number of last values to average
      Filters are useful for drawing charts, dynamic images, to eliminate jerks and jumps
  - <break/> decorator allows you to split rendering and start a new independent stage
  - New attributes in sections:
    maximize-focus="true/false" - removes all optional controls
    controls="autohide" - forcibly hides control buttons
  - time-scale - in charts can compress the time axis by a multiple
  - Acceleration sensor is taken from the phone sensor. Direction detection.
  - Improved Power screen.
  - Experimental dash panel "DashB3"

- v1.8.8 20/08/2025
  - Android: screen boundaries on A15/16.

- v1.8.6 02/08/2025
  - Android: Target version 36
  - Main screen loading accelerated, screens are loaded on demand - saves memory and time
  - Decorators can now be declared more conveniently, via nested elements. For example
      <item id="Speed">
        <image image-zorder="bottom" image-path="images/dashboard-svgrepo-com.svg" image-scale="0.5"/>
        <image image-zorder="bottom" image-path="images/arrow.png" image-scale="0.5"/>
      </item>
 
- v1.8.5 24/06/2025
  - Gear shift assistant on the main screen (can be disabled via settings)
  - Fixed overlay of parking indicator on the gear indicator
  - ECU: 
    Toyota Prado 150 AT+Engine CAN mode21
    SsangYong Korando2 G15DTF Engine+AT+CAN


- v1.8.4 __/04/2025
  - ECU: 
    Toyota Prado 150 AT+Engine CAN mode21

- v1.8.3 06/04/2025
  - Removed dynamic temperature indicators - many users did not like it
  - Estimated fuel consumption - improved calculation algorithms.
  - Layouts:
    - added "if" parameter for conditional display
    - fixes in stack-vertical, stack-horizontal
  - Main screen settings: You can change the order of screens, and select screens that will always be displayed
  - Some screens now have the ability to customize display parameters:
    - display the remaining fuel calculated or from the ECU
    - scroll through temperature sensors or display them all together (by default, the old behavior is restored - temperatures side by side)
    - display average speed without idle time or overall average speed
  - You can configure one custom screen with your own sensors (currently with a fixed layout)

    - v1.8.2 26/01/2025
  - Fixed jumping values on the missed trip screen
  - Engine total runtime screen (EngineHours)

- v1.8.1 01/01/2025
  - Android: the app works and launches even if not all permissions are granted.
  - Fixes in USB ELM adapter selection
  - Fixed HUD mode

- v1.8.0 18/11/2024
  - Sensor background recording control form
  - Online analysis of recorded sensors
  - ECU:
      Lancia VoyagerRT Diesel+AT+TPMS_CAN
  - iOS improvements and fixes in online analysis

- v1.7.18 02/09/2024
  - Android 14: fixed crashes on startup due to permissions

- v1.7.17 01/09/2024
  - iOS: data management dialog, small fixes
  - Android - fixed new issues in statistics.
  - Cloud upload and backup are now more stable.

- v1.7.16 26/08/2024
  - iOS: BTLE adapter compat improvements
  - Connection status now shows red when ELM connection fails, and yellow when Vehicle connection fails
  - Sensor info dialog now shows all intermediate sensor values
  - Android: API 34
  - ECU: 
    Opel VB ABS(430) pin12
    Micas 11CR  ГАЗ

- v1.7.15 27/05/2024
  - Androd: Autohide buttons
  - New: background cloud data sync and backup
  - Data management new dialog
  - "Unavailable" on custom skins fix
  - Fix hanged permission query on start
  - ECU: 
    VW Touareg 7L 2.5tdi BAC
    SsangYong Rexton G4 D22DTR+AT+TPMS+OBD

- v1.7.13 09/03/2024
  - Fixes

- v1.7.12 02/03/2024
  - ECU: 
    Chery A21 ABS(TRW)

- v1.7.11 28/02/2024
  - Fixes for very long displays
  - RuStore fixes
  - ECU: 
    Chery A21 ABS(MK70)

- v1.7.9
  - ECU: 
    SsangYong Kyron AT4 (11pin)

- v1.7.8
  - config.xml: new <store-state-interval>

- v1.7.7 11/26/2023
  - Statistics: new dark theme

- v1.7.6 11/11/2023
  - Android: on start hobdrive will use currently selected theme (day/night)

- v1.7.5 11/05/2023
  - Fixed Bluetooth permissions on Xiaomai

- v1.7.4 11/01/2023
  - Fixed stats sorting bugs

- v1.7.0 10/07/2023
  - User folder and files support improvement: "override" folder
    User files are read from any 'override*' and also from root folder.
  - Fixed MIL description duplication

- v1.6.34 09/26/2023
  - Android:
    - Fixed status updates for latest androids
    - Notice text can be customized, for now only with "<custom-notification>",
      'eval' syntax should be used.

- v1.6.33 09/22/2023
  - Android
    - New USB support library with CDC, Ch34x
    - when removing app, you can choose to keep its settings
    - RuStore improvements
  - UI: hobdrive now aligns better font sizes
  - Stats and events: improved sorting and fixing bugs

- v1.6.32 06/15/2023
  - Android: setting to disable HUD button
  - iOS - fixed crash in Fueling dialog

- v1.6.30 01/22/2023
  - improvements in fuelings and events lists.
  - statistic generation is eliminated from app
  - Adaptations for Android 12 (sdk 31)

- v1.6.29 12/19/2022
  - obfuscar changes
  - 'override-by-user' system parameter in config.xml, allows overrider
    any system file with same file in user folder
  - Android11 in stats fixes

- v1.6.28 05/18/2022
  - internal changes

- v1.6.27 04/30/2022
  - ECU: 
    Chery Tiggo8 OBD+TPMS CAN
    IKCO Samand CNGunits SiemensCI4 (DTC only)
    KIA Ceed 1.6 G4FG without TPMS

- v1.6.26 02/11/2022
  - ECU: 
    Mitsubishi iMiEV Li-ION add "Capacity Measure"
    GreatWall H5 4D20 ENG+AT+IP+OBD Can
    GreatWall H5 4D20 ENG+AT+IP+ABS+OBD Can
    Haval H5 Instrumental Panel+OBD2
    Haval H5 Instrumental Panel+ABS+OBD2
    SsangYong  ABS (Can)
    Renault MeganeIII K9K636_X95
    Chery S11(QQ) Magneti Marelli 472
    Hyundai Elantra MD 1.8 MT+TPMS+ECM+OBD
    Hyundai SantaFe TM G6DU 3.5+AT+TPMS CAN
    KIA G6BA KWP (SportageKM 2.7)
    KIA Soul PS (USA) G4FD GDI+AT+TPMS+OBD
    Suzuki JDM ENG+AT+TPMS CAN
    Infiniti EX25-QX50 (J50) TPMS+OBD CAN
  - Add Gaz Trigger - Primary fuel switch voltage Oxygen B1S2

- v1.6.25 12/07/2021
  - Fix bug 

- v1.6.24 11/27/2021
  - Fix bug Gaz Trigger

- v1.6.23 11/15/2021
  - ECU: 
    Add CDPF pids to SsangYong D20DTF+DSI6+OBD CAN 
    SsangYong RextonW D20DTR+AT5+OBD CAN
    Hyundai Grand Starex TQ D2.5+AT+4WD+TPMS
  - Ability to use various variables in layout expressions:
      Pi E PositiveInfinity NegativeInfinity
      All variables from gauge (Color_white Color_red, ...)
      daynight layout
      Variables from the theme (Color_normal Color_contrast Color_minor Color_subtle Color_back Theme_brightness)
      Variables with vehicle data (Vehicle_tank, Vehicle_tank_secondary, ..)
      The AllGlobals () function returns all available global variables and their values
  - Attributes min-limit and max-limit fix the lower and upper bounds of the value for drawing charts
  - Fixed some bugs in graph drawing
  - In the refueling dialog, the fuel cost is displayed with two decimal places
  - Android:
    Now, if there is Wifi and a mobile network, the connection to the adapter is carried out via a wifi network.
    This allows both networks to be used at the same time. 

- v1.6.22 08/17/2021
  - ECU: 
    KIA K7 3.0LPI ECM+AT+TPMS+OBD
    KIA Optima TF(QF) 2.4GDI+AT+TPMS_CAN+AT+TPMS_CAN

- v1.6.21 08/11/2021
  - ECU: 
    KIA Carens(UN) D4EA AT+CAN
    Toyota ABS only(ISO14230)
    Toyota Auris/Corolla 1GR-FE+MMT
  - Extended date formatting
  - Total data reset: cleans only user statistics, but not config


- v1.6.20 06/19/2021
  - ECU: 
    UAZ Patriot 17.9.71 CAN VS35
  - Reply verificaiton in HFM protocols

- v1.6.19 06/13/2021
  - ECU: 
    Toyota Auris/Corolla 1GR-FE+MMT
    Toyota Auris/Corolla MMT
    Toyota Auris/Corolla EMPS
  - Android: Dialogs are now following android day/night theme

- v1.6.18 06/05/2021
  - Android fixes

- v1.6.17 06/02/2021
  - ECU: 
    Hyundai SantaFe TM D2.2+8DCT+TPMS CAN
  - Android fixes

- v1.6.16 05/25/2021
  - ECU: 
    SsangYong SSPS/EPS (12pin)
    SsangYong SRS (9pin) onlyDTC
    Hyundai G4KE+AT+4WD CAN (SantaFe CM)
  - Android:
     Changes to Google Play requirements, file storage is migrated from
     hobdrive (hobd) folders at the root of the map - to the Android / data / hobdrive.android / files folder
     Also, many of the system files that were previously available in this folder
     are now hidden. The folder contains only user settings. 
  - Android: Sensor widgets settings now can take any free text (for expert users)

- v1.6.15 03/28/2021
  - ECU: 
    Nissan kwp AT (only) InfinityAT S50
    Infiniti QX70(S51) Consalt3+AT+TPMS
    Hyundai G4KJ 2.4 GDI (Sonata YFA)
    Hyundai Sonata YFA G4KJ+AT+TPMS_CAN
  - Android:
    Location tracking is now done only in active mode

- v1.6.14 02/24/2021
  - ECU: 
    Infiniti FX35(S51) FX50 TPMS+OBD CAN
    Infiniti FX35(S51) FX50 AT+Consalt3+TPMS+OBD
    SsangYong Stavic D20DI(DT)+OBD CAN
    SsangYong Stavic AT+OBD
    SsangYong Stavic D20DI(DT)+AT+OBD CAN
    SsangYong AT5(5G-Tronic, 722.6) (CAN, only AT)
    Hyundai Elantra AD 2.0 TPMS+MT+Panel+OBD
    Hyundai IX35 G20MPI+AT+TPMS+4WD+OBD
    Geely Atlas NL-3 ENG+AT+TPMS+4WD+OBD
    Suzuki JDM ENG+TPMS CAN

- v1.6.13 12/18/2020
  - ECU: 
    Renault MeganeII K4M INJ_S 3000_A7A3_48_A
    Renault SCENIC II INJ_SID301+PANEL+UCH+UPC
    Renault Duster ph2 EMS3155_D6_44_UDS+AT+4WD+AIRBAG+UCH+HANDS-FREE+PANEL
    Renault MeganeIII SID305 K9K832_836_X95+PANEL+UCH+CONDITIONING+VARIABLE+UPC+DOOR
    Renault FLUENCE K4M(INJ_V40)+AT(DP0_TA20005_L38_B32)
    Infiniti FX35(S51) FX50 AT+Consalt3+OBD
    KIA Sorento(UM) Prime G4KJ AT+4WD+TPMS+OBD
    SY AT(Powertech 6th)
    SsangYong Actyon G20D Engine+AT+4WD
  - Update HFM MIL Sensors


- v1.6.12 10/02/2020
  - ECU: 
    KIA Soul PS Nu20MPI+AT+OBD
    KIA Cerato YD G4FG 1.6+AT CAN
    KIA Cerato BD N20NU+AT+TPMSbyABS+OBD

- v1.6.10 09/09/2020
  - ECU: 
    Fiat Panda (Magneti Marelli IAW 4AF/4EF/59F/5AF)
    Hyundai Tucson TL 2.0MPI+AT+4WD+TPMS
    Hyundai Accent(Solaris) HCR 1.6 G4FC ECM+TPMSbyABS+OBD
    Hyundai G4LC_KAPPA+AT+TPMS_by_ABS CAN (RIO FB)
    Hyundai Elantra XD 2.0 Eng KWP
    Hyundai Elantra XD (AT only)
    KIA Seltos CVT+TPMS+OBD
    Nissan X-Trail T32 CVT+OBD
    Nissan X-Trail T32 CVT+Consalt3+OBD
    Nissan Consult III ENG+OBD
  - Update HFM MIL Sensors

- v1.6.8 06/30/2020
  - ECU: 
    Hyundai G4LC_KAPPA+AT CAN (RIO FB)
    Hyundai Accent/Solaris RB 1.6 G4FC AT+ECM+OBD
    Hyundai Accent/Solaris RB 1.4 G4FA AT+ECM+OBD
    Hyundai G4KE+AT+4WD+TPMS CAN (SantaFe DM)
    Hyundai SonataYF i45 G4KD+AT_CAN
    KIA Sportage QL 2.0MPI(G4NA) 4WD+AT+TPMS+OBD
    KIA TPMS(only) 8pin KWP
    Nissan Note e-Power 2016-08.17
    Nissan ABS (only DTC KWP)
    Toyota Corolla CAN21 (2018)
    Volkswagen Tiguan 1.4 TSI CAVA
  - Add 9 gears in settings
  - Added GPS coordinates record when adding a new refueling and a new record

- v1.6.7 04/30/2020
  - ECU: 
    Hyundai/KIA G4EE+AT CAN (Accent,RIO)

- v1.6.6 04/29/2020
  - ECU: 
    Hyundai D4HB+AT CAN(SantaFE) добавлен Reset ECU AT
    Hyundai G4KE+AT CAN (SantaFe DM)

- v1.6.5 04/23/2020
  - Changed position of buttons in "Manage profiles" 
  - Added a warning popup when deleting a profile
  - ECU: 
    KIA Ceed AT+TPMS+OBD add Reset ECU AT
    KIA Ceed AT+OBD add Reset ECU AT
    Hyundai G4KD+AT CAN

- v1.6.4 04/01/2020
  - ECU: 
    Mitsubishi SRS only DTC CAN
    Mitsubishi SRS only DTC KWP
    KIA RIO(UB) ECM+AT+OBD CAN
    Hyundai Elantra(MD) EPS+OBD CAN

- v1.6.3 02/25/2020
  - ECU: 
    SsangYong AT4(BTRA) (11pin)

- v1.6.1 01/19/2019
  - ECU: 
    Mitsubishi_4N15+AT_V8AWG+TPMS+Panel+OBD
    Hyundai Sonata EF G6BA KWP+OBD
    Hyundai Sonata EF G6BA KWP (AT only)
    Elantra AD, Creta add ECU: "Smart Junction Block", "Panel"
    Hyundai G4ED+AT CAN add sensors


- v1.6.0 12/03/2019
  - Fixed bug with alert popup in Windows
  - ECU: 
    Mitsubishi 4N15+AT_V8AWG+TPMS+OBD CAN
    Elantra MD, Avante MD add ResetAdaptationAT
    Hyundai Creta2.0 4WD+AT+TPMS+ABS+ECM+OBD
    CitroenC4 ME745
    Renault ZOE
    KIA Optima NU20MPI AT+TPMS+OBD
    Mitsubishi MinicabMIEV Li-Titanate
    Hyundai Getz AT (only)
    SY DSL D20DT(C2I) E3 KWP, SY DSL D27DT(C2I) E3 KWP add Active test


- v1.5.50 09/19/2019
  - ECU: 
    Hyundai D6EA CAN+AT(IX55)
    Hyundai D4CB KWP (H1)
    Hyundai Elantra MD 1.8 AT+TPMS+ECM+OBD
    Hyundai Avante MD 1.6 GDI ECM+AT+TPMS+OBD
    Delphi MT22+OBD (Hover)
    SY DSLD20DT(C2I) E3 KWP (update)
    Mitsubishi 4D56+AT_V5A5A+OBD CAN
    Mitsubishi 4N15+AT_V8AWG+OBD CAN

- v1.5.48 07/16/2019
  - ECU: 
    Peugeot (EDC17C10_BR2) Partner - update
    
- v1.5.47 07/09/2019
  - ECU: 
    Mitsubishi MINICAB MiEV Li-ION 2014
    SY DSLD20DT(C2I) E3 KWP (update)
    Hyundai G4FA CAN (i30)
    Hyundai D4FA CAN (Accent)
    Hyundai TucsonJM G4GC KWP
    Peugeot (EDC17C10_BR2) Partner
    Opel Astra X16SZR
  - Fixed crash on new versions of android - during the update and send a message about the problem

- v1.5.46 04/23/2019
  - ECU: 
    Renault MeganeII K4M INJ_S3000 CAN
    update Mitsubishi iMiEV Li-ION and Li-Titanate
    Infiniti QX50 TPMS+OBD CAN
    Jeep WranglerJK_Diesel+AT_CAN
  - Fixed crash of the application during the selection of sensor settings

- v1.5.45 03/13/2019
  - ECU: 
    Hyundai Terracan 2.9d J3 KWP
    KIA Magentis 2.5 G6BV_Unl_ZrO2+OBD
    Opel Y20DTH Vectra C
    Hummer AT+OBD CAN
    Lifan UAES ME1788+OBD CAN
    SY DSLD20DT(C2I) E3 KWP
    SY DSLD27DT(C2I) E3 KWP
    Mitsubishi iMiEV Li-Titanate
  - Added the ability to enter custom values ​​in some fields, including currency 
  - In 'OBD-II Port Settings' on android, the popup of the USB connection request window has been fixed

- v1.5.44 01/03/2019
  - Restored GPS operation on the latest versions of android
  - Fixed bugs when working in the background
  - Changed UI in the sections "Diagnostics" and "OBD II Port Settings"
  - ECU: 
    Hyundai Elantra MD 1.6 AT+ECM+OBD
    Hyundai D4EA KWP (SantaFE)
    Hyundai TucsonTL20MPI 2WD+AT+TPMS+OBD
    KIA_Ceed GT 1.6T-GDI MT+TPMS+ECM+OBD
    Subaru SSM2 ENG+OBD CAN	
    Subaru SSM2 ENG+AT+OBD CAN
    Subaru JDM SSM2 ENG CAN	
    Subaru JDM SSM2 ENG+AT CAN
    Renault Laguna3 M9R ENG CAN
    Renault Laguna3 M9R ENG+Clima CAN
    Renault SCENIC II K9K X84ph1 722 728 729 CAN
    Renault SCENIC II INJ_SID301_00B1_44_A CAN
    Renault SCENIC II INJ_SID301+InstPanel CAN


- v1.5.43 11/14/2018
  - ECU: 
    Mitsubishi Outlander PHEV+OBD
  - bugfixes
  
- v1.5.42 11/09/2018
  - bugfixes

- v1.5.41 11/02/2018
  - ECU: 
    Alfa Romeo 156 ME7.3.1
  - Added function to automatically start hobDrive with the interface when the device is turned on
  - Starting with Android 6.0, permission is requested to write files to external memory of the phone

- v1.5.40 10/05/2018
  - ECU: 
    Ssang Yong Diesel E3 KWP add sensors C2I
    Ssang Yong AT5(T-Tronic) (11pin)
    Ssang Yong DSI 6th (only)
    Hyundai Creta1.6 ABS+OBD
    Hyundai Creta1.6 4WD+MT+TPMS+ABS+ECM+OBD
    Hyundai G4GC CAN

- v1.5.39 09/18/2018
  - ECU: 
    Ssang Yong 4WD(TOD) (13pin)
  - Fix crushing on android < 4.3

- v1.5.38 09/13/2018
  - ECU: 
    Ssang Yong TEVES ABS (8pin)
    Ssang Yong TEVES ABS/ESP (8pin)
    Suzuki_JDM_CAN
    Hyundai D4HB CAN+AT(SantaFE)
    Hyundai G4ED CAN
    Hyundai G4ED+AT CAN

- v1.5.37 08/09/2018
    - Fixed launch crash on android 7.0
   - Fixed crash when resetting errors from the Diagnostics section
   - Fixed black screen with USB connected
   - Added events for resetting intervals - Auto Trip, Trip A, Trip B

- v1.5.36 07/11/2018
  - ECU: 
    Renault DUSTER ph2 INJ_SID306
    Hyundai_D4EB_CAN
    Mitsubishi iMiEV

- v1.5.35 05/19/2018
  - Introduced a 5-day demo mode, in case of an unsuccessful license check of Google Play
  - ECU: 
    Alfa Romeo 156 M1.5.5
    Dodge Journey Diesel Can
    Hyundai Tucson1.6T-GDI AT+OBD CAN
    Hyundai Tucson1.6T-GDI 4WD+OBD CAN
      Hyundai Tucson1.6T-GDI TPMS(7D6)+OBD CAN
      Hyundai Tucson1.6T-GDI 4WD+TPMS+OBD CAN 
    Hyundai Tucson1.6T-GDI AT+4WD+TPMS+OBD CAN
    Toyota RAV4 XA40 CVT

- v1.5.34 05/15/2018
  - ECU: 
    KIA D4HA CAN (Sportage)
    KIA KJ29CRDI (Carnival2)
    Opel Mokka AT+OBD CAN
    Chery S11(QQ) MT Siemens 472
    Subaru TPMS(only) KWP
    VAZ Январь 5/7 LS var2(0D)

- v1.5.33 03/14/2018
  - ECU:
    Chrysler(Dodge, Jeep) Voyager V Diesel Can
  - Improved TPMS Settings
  - Fixed calculation of fuel consumption for double fuel configuration

- v1.5.32 03/07/2018
  - ECU:
    Ssang Yong Diesel E3 KWP add sensors
  - Fixed simultaneous use of speed correction coef with the 'Cut bad sensor values (Speed, RPM)'

- v1.5.31 02/06/2018
  - ECU: 
    Renault Duster INJ_H4M_X38_EMS3120 CAN
    Renault Fluence K4M INJ_V40 CAN
    Delphi MT20U add "IdleAirControl"
    Ssang Yong Diesel E4 KWP
    Ssang Yong Diesel E4+OBD2 KWP
    Hyundai SRS (Can)
    Hyundai Creta1.6 4WD+AT+TPMS+ECM+OBD
      Hyundai Creta1.6 4WD+MT+TPMS+ECM+OBD
      Hyundai Creta2.0 4WD+AT+TPMS+ECM+OBD
    Ford Mondeo J1850
    Nissan Skyline V36
    KIA_Ceed AT+TPMS+OBD
    Opel Z13DTJ
    Micas 11.4 ZAZ+OBD2
  -	Fixed TPMS indication icons in different ranges
  -	Fixed crash app when changing profile from diagnostic section
  -	You need to exit the application android 1 time, instead of several
  -	Custom units are correctly saving
  -	Correctly calculates the gas consumption for a dual-fuel configuration

- v1.5.30 12/24/2017
  - ECU: Hyundai Tuscani
  - bugfixes

- v1.5.29 12/22/2017
  - ECU: Toyota1GR-FE, Chery M11: ABS, SRS; Hyundai Read/Clear DTC TPMS
  - Added refueling for dual-fuel configuration
  - Corrected the behavior of the fields in the 'ECU settings'
  - Correct display background picture when changing theme
  - Autocorrection when entering a float numbers

- v1.5.28 11/14/2017
  - Android bugfixes

- v1.5.27 11/12/2017
  - ECU: CitroenC5 ME747, CitroenC5 BVA_AM6_AT6
       EDC16C3_CAN: Peugeot 307 2007 1.6, Citroen Jumpy 
         SsangYong Rexton, Chairman - Gasoline
       UAZ 1797 MAP
       Hyundai TucsonTL20MPI AT_2WD
       Ford Fusion Duratec16V CAN 
  - Improved backup function
  - Fixed crash after saving settings
  - Correct calculate distance run in the double-fuel configuration 
  - Saving coef. speed correction from the 'wheel size configuration'
  - Improved 'Connection settings' pop-up window
  - Split settings into groups - UI, System, Expert
  - Added the opportunity to become a beta tester
  - View "Changelog" in android version
  - Highlighting the selected item in the list
  - Double tap 'back' closes application
  - Running shell commands from the application

- v1.5.26 09/11/2017
  - ECU:  Hyundai Elantra AD 1.6

- v1.5.25 09/06/2017
  - ECU:  Hyundai AD TPMS

- v1.5.24 09/03/2017
  - Increasing data stability when memory card fails
  
- v1.5.19 07/01/2017
  - Android bugfixes

- v1.5.18 06/21/2017
  - Android bugfixes

- v1.5.16 05/29/2017
  - hobdrive will display MPG without jams when real MPG is too big
  - Fine settings of display units
  
- v1.5.15 04/01/2017
  - Dual fuel configuration (Natural gas)
  - ECU: KIASportage4WD, Mitsubishi 4M41 (Pajero), Toyota 1ND-TV CAN, Suzuki_JDM_KWP01 (KEI), VAZ m86 CAN, Hyundai Starex,
    Renault MeganeIII/ScenicIII SID305 K9K832_836_X95, Hover 4D20 DelphiDCM 3.7, Renault CAPTUR INJ_F4R_H79ph2_EMS3125,
    Peugeot308_MEV17_4_2, KIA Optima AT, Chevrolet Captiva AT
  - Improvements in dialogs structure for vehicle settings and profiles.
  - Weekly backup on statistics
  - TinyExe: function svalid(Sensor), svaluef, svaluef_units
  - Layout: Nested grids, stack-vertical/stack-horizontal, union, switch - we now can make complex layouts
    Conditional elements display with 'visibility' attribute.

- v1.4.28 01/29/2017
  - Android: USB ELM adapters are now supported

- v1.4.27 01/29/2017
  - ECU: 
    - Renault Duster DCM34 K9K830, 4WD (only) CAN, K4M EMS3130 kwpFI.
  - Separate DTC descriptions on different ECUs. (MB HFM in particular)
  - Translation improvements for Portugal-Brazil.
  
- v1.4.26 12/21/2016
  - ECU: 
    - Toyota ABS (CAN)
    - Peugout 807 EDC15C2 
    - KIA AT Aisin

- v1.4.25 11/21/2016
  - ECU: 
    - Renault Megane 2 (CAN)
    - Renault Duster K9K Diesel (CAN)
    - Fiat PALIO/PUNTO (BoschME73H4_M)
    - Toyota SRS: MIL codes reading.
  - 'image' decorator supports new attributes 'image-width/image-height' - absolute image size in screen pixels
  - charts now support attribute cover-description (By default they'll not draw over sensor description text)
  - All Sensors list now could be filtered with sensor groups.
    You may setup filter explicitly in layout: <section name="Renault Sensors" class="SensorList" ns="Renault">
  - Gauge description can use special filter for all sensors on specific screen.
    Example: <gauge id="Sensor List:*" border-opacity="1"/> will set visible border for all sensors only on "Sensor List" screen
  - ECUXML:
    - <base-data> should be used to skip reply prefix bytes automatically, may be used instead of <base-raw>
    - <text offset="20" length="10">: text sensors
    - get_wordle/get_dwordle, to_signed_byte/to_signed_word/to_signed_dword in <eval>
    - data-offset attribute changes default offset of data bytes in OBD2 reply(default is 2 bytes)

- v1.4.24 10/10/2016
  - win32 build fixes
  - "Color description" (colored-descr in layout) new attribute. If ON, description text will be colored as sensor itself
  - New "padding" decorator. Controls borders of inner widget. (Use like: decorator-p="padding" p-padding="5 15 5 15").
    Offset units are logical pixels here.
  - ECU: Mercedes HFM_PMS. Mercedes C,E,S with M111\M104 93-98 year. You'll need custom ELM hardware from hfmscan.com

- v1.4.23 10/04/2016
  - Statistics screens layout improved. Visual tracking of values.
  - ECU: UAZ Bosch 17.9.71
  - Fixes for Android 5.х

- v1.4.22 09/16/2016
  - Fixed rare crash on start in case OBD2 device is not bonded.
  - New Efficiency data screen. Percentage efficiency shows how
    soft and efficient your driving is.
  - ECU: KIA RIO 2012 (UB) AT

- v1.4.21 08/22/2016
  - ECU: ABS Bosch 8(8.1) VAZ, UAZ
  - Fixed bug with lost connection detection on KWP protocols

- v1.4.20 08/19/2016
  - Android:
    - Fixes on hobdrive start with corrupted data
    - Increased WiFi on/off interval for more stable wifi reconnect
    - Fixed crash on some devices

- v1.4.19 07/20/2016
  - ECU: Bosch EDC15C2 (Citroen Jumpy 2.0HDI RHZ)
  - ECU: Toyota D-4D KWP (Hilux)

- v1.4.18 07/14/2016
  - Strict OBD2 conformance tests
  - Android: Fixed rare error on ugly chinese kernel images

- v1.4.17 07/13/2016
  - WP8.1: Rare fixes, hangouts
  - WP10: Fuxes with window scaling

- v1.4.14 07/05/2016
  - ECU: Mercedes W169: Engine, CVT, ESP.
  - ECU: Hyundai_D4HB_CAN (SantaFe), Hyundai_G4EE_CAN (Verna, Accent)
  - ECU: Geely MK Cross Delphi MT20U2
  - ECU: SsangYong DSI_6th
  - ECU: SsangYong D20DTF (Actyon New, Korando C)
  - ECU: MB C-Class W203 KWP CR3_UP
  - ECU: Toyota Prius 30
  - ECU: KIA Ceed TPMS
  - ECU: Micas 7.6, 10.3, 11.3 fixes
  - Improvements in performance screens:
    - Speedups by various intervals.
    - Slowdowns from explicitly selected speed.
    - Settings for power calculationsß.
  - Android BT: connection will go in reset when bluetooth transport errors are detected

- v1.4.8 05/15/2016
  - ECU: Scale fixes in cycle consumptions
  - ECU: MBw168 Errors reading/reset
  - ECU: ABS Chery Tiggo errors read/reset 
  - ECU: Yanvar, delphi-mt20u, Micas ECU reset
  - ECU: Yanvar, Micas - Initial design on Invoke Mechanisms. Vent switch on/off
  - ECU: fixes on MB Sprinter
  - When changing trip modes, screens etc, hobdrive now shows all sensor updates instantly, without delays.
  - Android: Stability improvement when using SD card storage.
  - Android: Improvements in license handling from play market. Option to manually prolongate license (revalidate it).

- v1.4.6 04/08/2016
  - ECU: MB w168, Hyunday G4FC, Solaris, Ford kline fixes, Peugeot 807, KIA RIO 2003-05, Suzuki(JDM, H27) .
  - "Naruto" TPMS - chinese tire monitoring bluetooth module support
  - Android: Sometimes connection status in top bar was invalid
  - Connection status now shows counter bar until next retry

- v1.4.5 02/28/2016
  - Исправления в конфигурации андроид версии - выставлено явное требование андроид 3.x

- v1.4.4 02/28/2016
  - ECU: Sagem2000 (Citroen), fixes in sirius, renault g4ee.

- v1.4.3 02/18/2016
  - Fixes in working with custom headers
  - Fixes with DTC clearing in Sirius ECU

- v1.4.2 02/03/2016
  - Diagnostics screen missed MIL codes with very long descriptions.
  - Improved some skins quality
  - Generic fixes
  - ECU:
    - Some toyota engines gone down because of extra pids reading by hobdrive.
    - Separate ECUs added for hybrid prius10.
    - Various ECU fixes.
  - Android:
    - Minimum platform version now: 4.0.4
    - Some devices had numeric keyboard without dot.
    - Added .nomedia files to keep hobdrive media files hidden for galleries.
    - Dialogs improved: OK/Cancel buttons are now in top action bar
    - OBD2 settings dialog improved. New options: use GPS, elm delay, disable elm spaces.

- v1.2.18 11/12/2015
- v1.2.17 11/12/2015
  - Android: improvements in orientation changes.
    Fixed scaling with dfferent settings in options.
  - ECU: standard-header is now correctly applied to all sensors in module.
    Bad replies (7F) are now recognized.
    There will be no fictive MIL code in case 7F is replied to MIL request.
  - Screen Tank Details now joins fuel level and fuel remainder.
    New gauge attribute is used - "text-evaluator".
  - Decorator 'image' now supports dynamic attributes. Example:
      image-rotate='$${ Sensor_Value - 45 }'
    image will be rotated depending on base sensor.
      image-path='images/gauges/rpms/$${ceiling(Sensor_Value/1000*2)}.png'
    image will be chosed depending on sensor value.
  - ecuxml:
    New tags <mode> and <command> for explicit OBD2 modes.
    New tag <eval> setups a custom expression for sensor value.
    Functions available: get(offset), get_word(), get_dword().

- v1.2.16 10/28/2015
  - WP8: Bugfixes, random crash fixes
  - WP8: Option to scale fonts if needed (in UI settings)
  - Animation and drawing performance improvements.
  - ECU: Bad MAF values fixes.
  - ECU: Mazda Capella, improvements in Nissan kline.

- v1.2.15 10/09/2015
  - ECU: Daihatsu ECU fix
  - Fixes in bad ELM handling errors on KWP protocols
  - Maps in stats are now displayed correctly
  - WP8: Help area is now working

- v1.2.14 08/08/2015
  - ECU: Mitsubishi CVT,  BoschEDC15_CF3_Diesel, ssangyoung fixes 
  - WinCE version should no more hang in memory after app close
  - Android: Allows to force custom locale in app
  - By default GPS position is in tracking data now.
  - GPS sensors on windows gave technical decimal degrees,
    now they are converted and displayed in normal floating point degree value.
  - New functionality with statistics generator. (Actions -> Generate/open statistics)

- v1.2.13 03/06/2015
  - New protocol: Ssang Yong Diesel
  - Attention to Ford owners: we've splitted Ford ECUs, now owners of Ford CAN vehicles (> 2005 year)
    should explicitly change protocol to "Ford CAN".
    Ford MSCAN protocol could be used to read MSCAN PIDs, but this requires hardware modification of OBD2 socket.
  - OBD2 buffer size increased to 2048b
  - In case hobdrive sees no reply from ELM, it'll now try a couple of requests and only after that disconnects.
  - In case there is no connection to ELM, hobdrive increments the interval between connection retries.
  - Element 'button' now takes 'action' attribute. F.e. action="run(winamp.exe)"

- v1.2.11 08/04/2015
  - New ECUs: subaru_jdm, Renault Scenic gas, Fiat Ducato (DieselBoschEDC16_CF4)
  - VAZ: error statuses, modes.
  - Fixes in VAZ, Itelma, Bosch ECUs.
  - Improved working with bad adapters, loosing parts of long KWP replies.
  - text-align: allows to align text on any side of the display.
  - color-map: allows custom color mapping, not only red-green line.
  - Custom expression evaluator in gauge/layout. Syntaxis is: ${2+2*2},
    see gauge files for more examples.
  - Android: Fixed issue with old MTK chips. "Legacy" connection mode is available for this in connection settings.

- v1.2.10 09/02/2015
  - Profiles dialog is finally fixed
  - Current attribute values for sensor UI config are now displayed
  - Fixed crash in "zero to hundred" screen on some non-standard configurations.
  - Hybrid screen is now visible only for hybrid ECUs.
  - New attribute text-values - shows text values mapping according to some rules. See examples in default.gauge
  - Fixed issue with periodic failure when drawing icons.
  - Status panel now popsup on connection error, and hides on successful connection.
  - Partial support for custom units setup. Expert settings, "user-units" - value is a list of user required units to be used.

- v1.2.8 19/01/2015
  - screens from different ECUs are now in separate files
  - ECU: Opel Astra-G
  - Fixed hangup on reconnection of KWP2000 protocols
  - New FE calculation method: Cyclomatic sensor (some diesels)
  - Android: Fixed error on selection from profiles
  - Android: Fixed error with secondary license checks

- v1.2.5 19/11/2014
  - Android: support adapters without bonding

- v1.2.4
  - Static area, with options to display date/time permanently.
    This panel also shows errors/warnings statuses
  - Initial implementation for Tire pressure control systems (Enfill tpms is supported)
  - Settings menu layout improvements

- v1.1.98 07/05/2014
  - Daihatsu ECU
  - Separate Micas 10.3 E2/E3

- v1.1.97 18/04/2014
  - Hungary language
  - Toyota JDM fixes
  - Protocols:
    AlfaRomeo(draft)
    SsangYong Diesel
    Yanvar J5LS
    Toyota Hybrid+CAN21 (joined)
  - Fuel trip algorithm chooser in vehicle settings: Detection by FSS, TimingAdvance or STFT sensor
  - Protocol patches are now available
  - Android:
  - Performance improvements
  - Problem fixes in 2D GPU Acceleration mode
  - Back key is now handled in app as well
  - All dialogs now contain explicit question marker for item help
  
- v1.1.96 02/04/2014
  - Android:
  - Fixes for dual-core intels (fonepad, tab3, etc)
  - Fixes for ART mode (android 4.4)
  - Option to choose day/night theme start and end hour
  - Option to setup data popup in notification panel
  - Autostart now works with 'quickboot' mode devices

- v1.1.94 04/02/2014
  - low end devices performance improvements
  - Yanvar ECU improvements
  - Interface improvements
  
- v1.1.92 15/01/2014
  - Bugfixes

- v1.1.91 13/12/2013
  - Sensor info dialog improvements.
    Extra actions are available for each sensor gadget ("actions" attribute).
    New action "go(General)" will move you to the specific screen
    New action "run(winamp.exe foo.mp3)" will run the specified application (windows)
  - Sensor info dialog is now displayed on long key press
  - Decorators for screen elements: "touch-progress" and "tile" (see theme.gauge)
  - Fuel trim on deceleration is now detected even on high speeds
  - New dialog to enter initial odometer value
  - New calculated sensor - current shift position.
  - New dialog to setup wheel/rims parameters.
  - New ECU profile for Январь5.1
  - Toyota gt86 profile
  - Ford Escape Hybrid ECU improved.
  - Hybrid new UI screens added
  
- v1.1.90 05/10/2013
  - Bug fixes

- v1.1.87 28/08/2013
  - android fixes: license management improvements
  - Changes in popup screens handling
  - Last minute data are now saved even on unexpected power off.

- v1.1.86 23/08/2013
  - Bug fixes
  - Android 4.x UI is improved in dialogs

- v1.1.85 20/08/2013
  - Stability improvements with data saving

- v1.1.84 19/08/2013
  - Support MIL reading in Opel via KWP
  - Support Mitsubishi MUT proto (draft)
  - New dialog to enter missing trips
  - Automatic missing trip calculation when entering a new fueling
  - Improvements in fueling dialog (full tank, custom units)
  - Popup action screens now consume full screen space

- v1.1.82 03/07/2013
  - "custom-description" allows to change sensor description
  - new values for custom-units: time12, timesec12 - for 12 hours format
  - Options for font size adaptation disabling
  - Ability to output extra sensor data as id="sensor[extradata]". For instance TripTime[fillup] always gives trip time on "Fillup" interval, independently of currently selected interval.
  - Improvements in Toyota21 ECU

- v1.1.78 29/06/2013
  - Android: Fixed resets on some HTC devices
  - CAN ERROR from ELM is now ignored
  - Lambda level3 PIDs, Cat PIDs.
  - Automatic backup of config&data in case of data failure
  - Improvements in BT search logic, in auto search screen

- v1.1.76 23/04/2013
  - HUD mode improvements
  - Fixes in reconnect timeouts
  - Windows: before sleep hobdrive tries to drop the connection link
  - Android: All dialogs now inherit orientation preference
  - Android: GPS search fixed (not worked on some devices)

- v1.1.74 11/04/2013
  - Extensions in delphi mt20u2
  - Sensor labels are now size-adapting
  - Devices with numeric keyboards can now switch screens. Each numeric key is an index of activated screen.
  - Improved translations: ua, arabic, portugue  -brazil, others

- v1.1.73 06/04/2013
  - Improved vehicle profiles
  - Bug fixes

- v1.1.70 27/03/2013
  - Fixes in protocol auto-search screen.
  - Ecuxml: allow custom header specification (prius.ecuxml, feh.ecuxml)
  - Android: System android option "Development -> Force GPU Rendering" causes
    screen shifts and flickering. The recommendation is to disable this option.
  -
  
- v1.1.68 17/03/2013  
  - Android: WiFi restarts only when network connection error
  - Improvements in "Trip" panel, new panel "Actions"
  - New actions "Total data reset" and "UI settings reset"
  - Improved popup sensor information window
  - Sensor UI setup is available directly from sensor information popup
  - Autosearch protocol and adapter screen
  - Improved layouts, charts settings. Fixes in occasional text overlapping
  - Bug fixes
  
  - New ECUs:
  - Ford Escape Hybrid
  - Toyota JDM CAN (> 2006)
  - MIL codes in toyota JDM
  - Nissan Custom improvements, MIL reading
  - ecuxml new options: cutlow, cuthigh - allows value cutting on threshold.  

- v1.1.63 17/12/2012
  - Terms of use dialog
  - New version updates dialog
  - Option to reset WiFi network on start (Helps to connect WiFi ELM automatically)
  - Version checks once a week
  
- v1.1.60 30/11/2012
  - Bosch ME1797 (UAZ Hunter, Patriot)
  - Signed values in ecuxml

- v1.1.57 20/11/2012
  - Toyota mode21 (JDM) and Delphi MT20U2

- v1.1.55 11/11/2012
  - Slovak language added (thanks to maxman!)
  - Fixed a bug with incorrectly drawn background on first start
  - Sensor Details screen was improved

- v1.1.51 
  - Filter low MAF flow in consumption when engine is not running
  - GPS port speed available in settings
  - popup notifications on events
  - Improved German and Chinese translations.

- 1.1.49 
  - German translation added
  - Fuel, saved during fuel cut calculated incorrectly during warmups and fast speedups.
  - Speed correction coefficient now works even for non-OBD2 protocols
  - ECI Sirius d42 (Chevrolet Lacetti)

- 1.1.47 14/08/2012
  - Min and max values on charts

- 1.1.45 14/08/2012
  - acteco fixes

- 1.1.44
  - Suspend/Resume handling on WinXP
  - Acteco ME797 ECU Profile
  - windows: gps settings in connection dialog. GPS is disabled by default

- 1.1.40 07/06/2012
  - Improvements in Diag screen
  - "Performance" screen
