# USB Fingerprints

`dmesg` output from the node with the device attached

## USB to Serial 

Device connected to the C-Bus gateway

```
[  212.747055] Initializing XFRM netlink socket
[  216.353269] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
[  216.353384] IPv6: ADDRCONF(NETDEV_CHANGE): cali7746483033b: link becomes ready
[  265.013220] usb 1-1.3: new full-speed USB device number 3 using xhci_hcd
[  265.093354] usb 1-1.3: device descriptor read/64, error -32
[  265.289353] usb 1-1.3: device descriptor read/64, error -32
[  265.485217] usb 1-1.3: new full-speed USB device number 4 using xhci_hcd
[  265.589617] usb 1-1.3: New USB device found, idVendor=067b, idProduct=2303, bcdDevice= 4.00
[  265.589645] usb 1-1.3: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[  265.589654] usb 1-1.3: Product: USB-Serial Controller D
[  265.589662] usb 1-1.3: Manufacturer: Prolific Technology Inc.
[  265.657792] usbcore: registered new interface driver usbserial_generic
[  265.657844] usbserial: USB Serial support registered for generic
[  265.669944] usbcore: registered new interface driver pl2303
[  265.669999] usbserial: USB Serial support registered for pl2303
[  265.670068] pl2303 1-1.3:1.0: pl2303 converter detected
[  265.677489] usb 1-1.3: pl2303 converter now attached to ttyUSB0
[  275.921648] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
[  275.921794] IPv6: ADDRCONF(NETDEV_CHANGE): califc1100ef705: link becomes ready
```

|Parameter | Value |
|----------|-------|
|idVendor  | 067b  |
|idProduct | 2303  |
|bcdDevice | 4.00  |

Node discovery configuration

```yaml
            - name: "cbus"
              matchOn:
                - usbId:
                    class: ["00"]
                    vendor: ["0b7b"]
                    device: ["2303"]
```