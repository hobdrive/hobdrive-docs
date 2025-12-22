
## Typical Issues

### HobDrive cannot connect to the vehicle

You should distinguish between problems connecting to the ELM adapter (the indicator lights up red immediately, with a corresponding error message), and problems connecting the adapter to the vehicle (in this case, messages are usually issued: BUS INIT ERROR, UNABLE TO CONNECT).

Reasons for inability to connect to the adapter should be sought in incorrectly entered/not entered adapter pin code, hardware problems with the adapter or device.

When unable to connect to the vehicle: check that it is OBD2 compatible. If your vehicle is not OBD2, it or its ECU must be explicitly listed in the list supported by HobDrive (for example, Toyota JDM - right-hand drive Toyotas, Delphi mt20u - Cherry Tiggo, Yanvar, Bosch - for VAZ vehicles, etc.)

### Unstable operation, frequent disconnections

For USB adapters operating from mobile devices, a typical problem is lack of power.

Bluetooth ELM adapters operating through USB-BT dongles often suffer from poor quality of the latter.
