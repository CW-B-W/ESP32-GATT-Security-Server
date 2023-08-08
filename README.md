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

<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/fca29338-8e42-4819-bcff-85c50a6f82d8">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/c4bcda87-0bde-482d-9a1b-3997f150793e">

## Passkey Entry

### Configurations
Compared to the `Just Works` example, `ESP_IO_CAP_OUT` is used in this example.

The `IO Capability` of iPhone(Initiator) should be `Keyboard Display`, and the `ESP_IO_CAP_OUT` is used so that the `IO Capability` of ESP32(Responder) should be `Display Only`.  
All the abovementioned settings result in using `Passkey Entry` as the pairing method.

https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/blob/9dc3708ee45ac8469a9dfcc97cb43e9b1cb58a0a/examples/example_ble_sec_gatts_demo-passkey.c#L557

<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8">

### Pairing process
On ESP32, the passkey is shown on the log.
```
V (39919) SEC_GATTS_DEMO: event = 0x0e

I (39919) SEC_GATTS_DEMO: ESP_GATTS_CONNECT_EVT
V (40009) SEC_GATTS_DEMO: event = 0x04

V (40659) SEC_GATTS_DEMO: GAP_EVT, event 0x0b

I (40659) SEC_GATTS_DEMO: The passkey Notify number:123456
```

On iPhone, a dialog is shown

<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/039b11c9-b9b9-49d2-8b88-ebc95af4b659">

## Numeric Comparison

### Configurations
Numeric Comparison is only available under `LE Secure Connection`.  
Compared to the `Just Works` example, `ESP_IO_CAP_IO` is used in this example.

The `IO Capability` of iPhone(Initiator) should be `Keyboard Display`, and the `ESP_IO_CAP_IO` is used so that the `IO Capability` of ESP32(Responder) should be `Display YesNo`.  
All the abovementioned settings result in using `Numeric Comparison` as the pairing method.

https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/blob/9dc3708ee45ac8469a9dfcc97cb43e9b1cb58a0a/examples/example_ble_sec_gatts_demo-numeric.c#L559

<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8">

### Pairing process
On ESP32, the passkey is shown on the log.
```
V (18519) SEC_GATTS_DEMO: event = 0x0e

I (18519) SEC_GATTS_DEMO: ESP_GATTS_CONNECT_EVT
V (18609) SEC_GATTS_DEMO: event = 0x04

E (19329) BT_SMP: Value for numeric comparison = 712885
V (19339) SEC_GATTS_DEMO: GAP_EVT, event 0x10

I (19339) SEC_GATTS_DEMO: ESP_GAP_BLE_NC_REQ_EVT, the passkey Notify number:712885
```

On iPhone, a dialog is shown

<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/4ae31f30-c5ec-4c84-b52e-d06ad28bae30">

## Out-of-Band (OOB)

It seems OOB cannot be used to pair with iPhone, iPhone seems to only support OOB with Apple accessories.

### Configurations
To try OOB pairing, the following are set
* `ESP_BLE_SM_AUTHEN_REQ_MODE = ESP_LE_AUTH_REQ_SC_MITM_BOND`
* `ESP_BLE_SM_IOCAP_MODE = ESP_IO_CAP_NONE`
* `ESP_BLE_SM_OOB_SUPPORT = ESP_BLE_OOB_ENABLE`
* `ESP_BLE_SM_ONLY_ACCEPT_SPECIFIED_SEC_AUTH = ESP_BLE_ONLY_ACCEPT_SPECIFIED_AUTH_ENABLE`

### Pairing process
If `ESP_BLE_SM_ONLY_ACCEPT_SPECIFIED_SEC_AUTH` is set to `ESP_BLE_ONLY_ACCEPT_SPECIFIED_AUTH_ENABLE`, the pairing always fails.  
This is the verbose logging.
```
V (17149) SEC_GATTS_DEMO: event = 0x0e

I (17149) SEC_GATTS_DEMO: ESP_GATTS_CONNECT_EVT
V (17239) SEC_GATTS_DEMO: event = 0x04

E (17689) BT_SMP: smp_proc_pair_cmd pairing failed - slave requires auth is 0xd but peer auth is 0x5 local auth is 0x5
W (17689) BT_HCI: hci cmd send: disconnect: hdl 0x0, rsn:0x13
E (17699) BT_BTM: btm_ble_remove_resolving_list_entry_complete remove resolving list error 0x2
D (17709) nvs: nvs_open_from_partition bt_config.conf 1
D (17709) nvs: nvs_set_blob bt_cfg_key0 216
D (17729) nvs: nvs_close 4
V (17729) SEC_GATTS_DEMO: GAP_EVT, event 0x08

I (17729) SEC_GATTS_DEMO: remote BD_ADDR: 4adfde648471
I (17729) SEC_GATTS_DEMO: address type = 1
I (17739) SEC_GATTS_DEMO: pair status = fail
W (17739) BT_BTM: btm_sec_clr_temp_auth_service() - no dev CB

E (17749) BT_BTM: Device not found

W (17749) BT_HCI: hcif disc complete: hdl 0x0, rsn 0x16
I (17759) SEC_GATTS_DEMO: fail reason = 0x50
I (17759) SEC_GATTS_DEMO: Bonded devices number : 0

I (17769) SEC_GATTS_DEMO: Bonded devices list : 0

V (17779) SEC_GATTS_DEMO: event = 0x0f

I (17779) SEC_GATTS_DEMO: ESP_GATTS_DISCONNECT_EVT, disconnect reason 0x16
V (17799) SEC_GATTS_DEMO: GAP_EVT, event 0x06

I (17799) SEC_GATTS_DEMO: advertising start success
```

If `ESP_BLE_SM_ONLY_ACCEPT_SPECIFIED_SEC_AUTH` is set to `ESP_BLE_ONLY_ACCEPT_SPECIFIED_AUTH_DISABLE`, `Just Works` seems to be used instead.  
This is the verbose logging.
```
V (7579) SEC_GATTS_DEMO: event = 0x0e

I (7579) SEC_GATTS_DEMO: ESP_GATTS_CONNECT_EVT
V (7699) SEC_GATTS_DEMO: event = 0x04

V (11519) SEC_GATTS_DEMO: GAP_EVT, event 0x09

I (11519) SEC_GATTS_DEMO: key type = ESP_LE_KEY_LENC
V (11519) SEC_GATTS_DEMO: GAP_EVT, event 0x09

I (11529) SEC_GATTS_DEMO: key type = ESP_LE_KEY_LID
V (11659) SEC_GATTS_DEMO: GAP_EVT, event 0x09

I (11669) SEC_GATTS_DEMO: key type = ESP_LE_KEY_PENC
V (11669) SEC_GATTS_DEMO: GAP_EVT, event 0x09

I (11669) SEC_GATTS_DEMO: key type = ESP_LE_KEY_PID
D (11679) nvs: nvs_open_from_partition bt_config.conf 1
D (11689) nvs: nvs_set_blob bt_cfg_key0 250
D (11699) nvs: nvs_close 4
D (11699) nvs: nvs_open_from_partition bt_config.conf 1
D (11699) nvs: nvs_set_blob bt_cfg_key0 263
D (11709) nvs: nvs_close 5
D (11709) nvs: nvs_open_from_partition bt_config.conf 1
D (11709) nvs: nvs_set_blob bt_cfg_key0 346
D (11729) nvs: nvs_close 6
D (11729) nvs: nvs_open_from_partition bt_config.conf 1
D (11729) nvs: nvs_set_blob bt_cfg_key0 406
D (11739) nvs: nvs_close 7
D (11739) nvs: nvs_open_from_partition bt_config.conf 1
D (11739) nvs: nvs_set_blob bt_cfg_key0 461
D (11759) nvs: nvs_close 8
D (11759) nvs: nvs_open_from_partition bt_config.conf 1
D (11759) nvs: nvs_set_blob bt_cfg_key0 475
D (11769) nvs: nvs_close 9
V (11769) SEC_GATTS_DEMO: GAP_EVT, event 0x08

I (11769) SEC_GATTS_DEMO: remote BD_ADDR: 4adfde648471
I (11769) SEC_GATTS_DEMO: address type = 1
I (11779) SEC_GATTS_DEMO: pair status = success
I (11779) SEC_GATTS_DEMO: auth mode = ESP_LE_AUTH_BOND
I (11789) SEC_GATTS_DEMO: Bonded devices number : 1

I (11799) SEC_GATTS_DEMO: Bonded devices list : 1

I (11799) SEC_GATTS_DEMO: 4a df de 64 84 71 
```

# References
*BLUETOOTH SPECIFICATION v4.2, Vol 3, Part H, 2.3 PAIRING METHODS*


<!-- <img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/0ada24d3-45d1-4142-bdbe-f5e302154760">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/fca29338-8e42-4819-bcff-85c50a6f82d8">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/c4bcda87-0bde-482d-9a1b-3997f150793e">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/81fc2beb-a528-4341-b7ca-51fb161cdba8">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/039b11c9-b9b9-49d2-8b88-ebc95af4b659">
<img width="25%" src="https://github.com/CW-B-W/ESP32-iPhone-GATT-Security-Server-Example/assets/76680670/4ae31f30-c5ec-4c84-b52e-d06ad28bae30"> -->
