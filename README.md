# ESP32-iPhone-GATT-Security-Server-Example

This repo is based on [gatt_security_server](https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/ble/gatt_security_server), and add more example including Just Works, Passkey Entry and Numeric Comparison.

# Examples
## Just Works
The official example [example_ble_sec_gatts_demo.c](https://github.com/espressif/esp-idf/blob/master/examples/bluetooth/bluedroid/ble/gatt_security_server/main/example_ble_sec_gatts_demo.c) uses Just Works to pair with iPhone.

In this example, `ESP_LE_AUTH_REQ_SC_MITM_BOND`, `ESP_IO_CAP_NONE` and `ESP_BLE_OOB_DISABLE` are used.

iPhone seems to disable OOB for devices other than Apple accessories, thus `OOB Data Flag` from iPhone is 0x00.  
Therefore when `ESP_BLE_OOB_DISABLE` is used, both `OOB Data Flag` is 0x00, OOB will not be used and `MITM` will be checked.  
And `ESP_LE_AUTH_REQ_SC_MITM_BOND` is used, the `SC` bit and `MITM` bit are set.  
Finally, the `IO Capability` of iPhone(Initiator) should be `Keyboard Display`, and the `ESP_IO_CAP_NONE` is used so that the `IO Capability` of ESP32(Responder) should be `NoInput NoOutput`.  
All the abovementioned settings result in using `Just Works` as the pairing method.

https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/blob/9dc3708ee45ac8469a9dfcc97cb43e9b1cb58a0a/examples/example_ble_sec_gatts_demo-justworks.c#L554-L562

<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/fca29338-8e42-4819-bcff-85c50a6f82d8">
<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/c4bcda87-0bde-482d-9a1b-3997f150793e">

## Passkey Entry
Compared to the `Just Works` example, `ESP_IO_CAP_OUT` is used in this example.

The `IO Capability` of iPhone(Initiator) should be `Keyboard Display`, and the `ESP_IO_CAP_OUT` is used so that the `IO Capability` of ESP32(Responder) should be `Display Only`.  
All the abovementioned settings result in using `Passkey Entry` as the pairing method.

https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/blob/9dc3708ee45ac8469a9dfcc97cb43e9b1cb58a0a/examples/example_ble_sec_gatts_demo-passkey.c#L557

<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8">

## Numeric Comparison

Numeric Comparison is only available under `LE Secure Connection`.  
Compared to the `Just Works` example, `ESP_IO_CAP_IO` is used in this example.

The `IO Capability` of iPhone(Initiator) should be `Keyboard Display`, and the `ESP_IO_CAP_IO` is used so that the `IO Capability` of ESP32(Responder) should be `Display YesNo`.  
All the abovementioned settings result in using `Numeric Comparison` as the pairing method.

https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/blob/9dc3708ee45ac8469a9dfcc97cb43e9b1cb58a0a/examples/example_ble_sec_gatts_demo-numeric.c#L559

<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8">

# References
*BLUETOOTH SPECIFICATION v4.2, Vol 3, Part H, 2.3 PAIRING METHODS*


<!-- <img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/0ada24d3-45d1-4142-bdbe-f5e302154760">
<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/fca29338-8e42-4819-bcff-85c50a6f82d8">
<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/c4bcda87-0bde-482d-9a1b-3997f150793e">
<img width="50%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8"> -->
