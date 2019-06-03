## Logitech mouse "Anywhere MX"

```
//button 1
0x00 0x00 0xc2 0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x3d
//button 2
0x00 0x00 0xc2 0x02 0x00 0x00 0x00 0x00 0x00 0x00 0x3c
//button 3
0x00 0x00 0xc2 0x04 0x00 0x00 0x00 0x00 0x00 0x00 0x3a
//button (no axis move) release
0x00 0x00 0xc2 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x3e

// axis -x
0x00 0x00 0xc2 0x00 0x00 0xff 0x0f 0x00 0x00 0x00 0x30
//axis +x
0x00 0x00 0xc2 0x00 0x00 0x01 0x00 0x00 0x00 0x00 0x3d
//axis -y
0x00 0x00 0xc2 0x00 0x00 0x00 0x10 0x00 0x00 0x00 0x2e
//axis +y
0x00 0x00 0xc2 0x00 0x00 0x00 0xf0 0xff 0x00 0x00 0x4f
//axis +z
0x00 0x00 0xc2 0x00 0x00 0x00 0x00 0x00 0x01 0x00 0x3d
//axis -z
0x00 0x00 0xc2 0x00 0x00 0x00 0x00 0x00 0xff 0x00 0x3f
```

Encoding:
```
byte 0:     not part of payload
byte 1:     device ID (in case frame is TXed with dongle address, instead of device address)
byte 2:     0xC2 == mouse report
byte 3-4:   always 0x00
byte 5:     LSBs of x-Axis (signed)
byte 6:     bit 7..4 LSBs y-Axis, bit 3..0 MSBs x-Axis
byte 7:     MSBs y-Axis
byte 8:     z-Axis (8 bit resolution)
byte 9:     always 0x00
byte 10:    checksum  
```


## Logitech Presenter "R400" (Sends unencrypted keyboard reports)

```
//Presenter key 0x4e
0x00 0x00 0xc1 0x00 0x4e 0x00 0x00 0x00 0x00 0x00 0xf1
//Presenter key release
0x00 0x00 0xc1 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x3f
```

Encoding:
```
byte 0:     not part of payload
byte 1:     device ID (in case frame is TXed with dongle address, instead of device address)
byte 2:     0xC1 == keyboard report
byte 3:     modifier (like in HID kbd default descriptor)
byte 4:     key1 (HID keycode, like in default kbd descriptor)
byte 5..9:  0x00 (maybe addtional keys, has to be tested)
byte 10:    checksum  
```


# Manual LED output via hidraw USB interface

```
echo -ne "\x20\x03\x0e\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00" > /dev/hidraw0 # all LEDs off
echo -ne "\x20\x03\x0e\x02\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00" > /dev/hidraw0 # all CAPS LED on
```

Resulting RF payloads for LED off (some additional bytes, which repeat)
```
0x00 0x0e 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0xf2
0x00 0x0e 0x00 0x00 0x11 0x00 0x00 0x00 0x00 0xe1
0x00 0x0e 0x00 0x81 0x07 0x00 0x00 0x00 0x00 0x6a
0x00 0x0e 0x00 0x81 0x00 0x00 0x00 0x00 0x00 0x71
0x00 0x0e 0x00 0x81 0x00 0x00 0x00 0x00 0xd8 0x99
0x00 0x0e 0x00 0x80 0x00 0x10 0x00 0x00 0x00 0x62
0x00 0x0e 0x00 0x81 0x07 0x00 0x00 0x00 0x00 0x6a
0x00 0x0e 0x00 0x80 0x00 0x10 0x00 0x00 0x00 0x62
0x00 0x0e 0x00 0x81 0x07 0x00 0x00 0x00 0x00 0x6a
0x00 0x0e 0x00 0x81 0x00 0x00 0x00 0x00 0x00 0x71
0x00 0x0e 0x00 0x80 0x00 0x10 0x00 0x00 0x00 0x62
0x00 0x0e 0x00 0x81 0x07 0x00 0x00 0x00 0xd8 0x92
0x00 0x0e 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0xf2
0x00 0x0e 0x00 0x00 0x11 0x00 0x00 0x00 0x00 0xe1
```

Resulting RF payloads for LED for CAPS on == 0x02 (some additional bytes, which repeat)
```
0x00 0x0e 0x02 0x80 0x00 0x10 0x00 0x00 0x00 0x60
0x00 0x0e 0x02 0x81 0x07 0x00 0x00 0x00 0x00 0x68
0x00 0x0e 0x02 0x80 0x00 0x10 0x00 0x00 0x00 0x60
0x00 0x0e 0x02 0x81 0x07 0x00 0x00 0x00 0x00 0x68
0x00 0x0e 0x02 0x81 0x00 0x00 0x00 0x00 0x00 0x6f
0x00 0x0e 0x02 0x80 0x00 0x10 0x00 0x00 0x00 0x60
0x00 0x0e 0x02 0x81 0x07 0x00 0x00 0x00 0xd8 0x90
0x00 0x0e 0x02 0x00 0x00 0x00 0x00 0x00 0x00 0xf0
0x00 0x0e 0x02 0x00 0x11 0x00 0x00 0x00 0x00 0xdf
0x00 0x0e 0x02 0x81 0x00 0x00 0x00 0x00 0x00 0x6f
0x00 0x0e 0x02 0x80 0x00 0x10 0x00 0x00 0x00 0x60
0x00 0x0e 0x02 0x81 0x07 0x00 0x00 0x00 0xd8 0x90
```