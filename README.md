# smart_usb_android

Android USB plugin for Flutter

## Usage

- [List devices](#list-devices)
- [List devices with additional description](#list-devices-with-additional-description)
- [Get device description](#get-device-description)
- [Connect device](#connect-device)
- [Check permission](#check-permission)
- [Request permission](#request-permission)
- [Open/Close device](#openclose-device)
- [Get/Set configuration](#getset-configuration)
- [Claim/Release interface](#claimrelease-interface)
- [Printer](#send)
- [Bulk transfer in/out](#bulk-transfer-inout)
- [Set auto detach kernel driver](#set-auto-detach-kernel-driver)

### List devices

```dart
await SmartUsbAndroid.init();
// ...
var deviceList = await SmartUsbAndroid.getDeviceList();
// ...
await SmartUsbAndroid.exit();
```

### List devices with additional description

Returns devices list with manufacturer, product and serial number description.

Any of these attributes can be null.

```dart
var descriptions = await SmartUsbAndroid.getDevicesWithDescription();
var deviceList = descriptions.map((e) => e.device).toList();
print('descriptions $descriptions');
```

**(Android Only)** Android requires permission for each device in order to get the serial number. The user will be asked
for permission for each device if needed. If you do not require the serial number, you can avoid requesting permission using:
```dart
var descriptions = await SmartUsbAndroid.getDevicesWithDescription(requestPermission: false);
```

### Get device description

Returns manufacturer, product and serial number description for specified device.

Any of these attributes can be null.

```dart
 var description = await SmartUsbAndroid.getDeviceDescription(device);
 print('description ${description.toMap()}');
```

**(Android Only)** Android requires permission for each device in order to get the serial number. The user will be asked
for permission for each device if needed. If you do not require the serial number, you can avoid requesting permission using:
```dart
var description = await SmartUsbAndroid.getDeviceDescription(requestPermission: false);
```

### Connect device

```dart
var connectDevice = await SmartUsbAndroid.connectDevice(device);
print('connectDevice $connectDevice');
// ...
```

### Check permission

_**Android Only**_

```dart
var hasPermission = await SmartUsbAndroid.hasPermission(device);
print('hasPermission $hasPermission');
```

### Request permission

_**Android Only**_

Request permission for a device. The permission dialog is not shown
if the app already has permission to access the device.

```dart
var hasPermission = await SmartUsbAndroid.requestPermission(device);
print('hasPermission $hasPermission');
```

### Open/Close device

```dart
var openDevice = await SmartUsbAndroid.openDevice(device);
print('openDevice $openDevice');
// ...
await SmartUsbAndroid.closeDevice();
```

### Get/Set configuration

```dart
var configuration = await SmartUsbAndroid.getConfiguration(index);
print('getConfiguration $configuration');
// ...
var setConfiguration = await SmartUsbAndroid.setConfiguration(configuration);
print('setConfiguration $getConfiguration');
```

### Claim/Release interface

```dart
var claimInterface = await SmartUsbAndroid.claimInterface(interface);
print('claimInterface $claimInterface');
// ...
var releaseInterface = await SmartUsbAndroid.releaseInterface(interface);
print('releaseInterface $releaseInterface');
```

### Printer

```dart
var send = await SmartUsbAndroid.send(bytes);
print('send $send');
```

### Bulk transfer in/out

```dart
var bulkTransferIn = await SmartUsbAndroid.bulkTransferIn(endpoint, 1024, timeout: 2000);
print('bulkTransferIn ${hex.encode(bulkTransferIn)}');
// ...
var bulkTransferOut = await SmartUsbAndroid.bulkTransferOut(endpoint, data, timeout: 2000);
print('bulkTransferOut $bulkTransferOut');
```

### Set auto detach kernel driver

Enable/disable libusb's automatic kernel driver detachment on linux. When this is enabled libusb will automatically detach the kernel driver on an interface when claiming the interface, and attach it when releasing the interface.

Automatic kernel driver detachment is disabled on newly opened device handles by default.

This is supported only on linux, on other platforms this function does nothing.

```dart
await SmartUsbAndroid.setAutoDetachKernelDriver(true);
```
