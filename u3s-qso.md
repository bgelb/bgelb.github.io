# Making 2-way JT9 QSOs with a QRP Labs U3S

![U3S Transmitter](u3s.jpg)

The heart of my apartment 630m "QRP" station is a [U3S Transmitter](https://www.qrp-labs.com/ultimate3/u3s.html) from QRP Labs. This kit transmitter is designed to be used as a QRP/QRSS beacon, rather than making QSOs. It works well as a WSPR beacon, but there's only so much fun to be had WSPRing.

The U3S has limited support for transmitting JT9 messages. The U3S can be configured with a 13-character "freeform" JT9 message, as well as a transmission schedule (how many minutes between "frames" and what minute of the hour to begin transmitting). Generally programming of the U3S is done awkwardly through various menus and a pair of momentary pushbuttons. This is fine for beacon use, but not practical for QSOs.

So to make QSO-mode operation possible, I've devised a hacky scheme to change the JT9 messages and transmit interval (selecting the "even" or "odd" minute for transmission) from my computer commandline. It has three parts.

- Reading and writing U3S EEPROM (which contains the U3S settings)
- Using the u3s-eeprom-tool to modify the EEPROM content with desired settings
- A simple shell script to automate the process

## Reading and writing the U3S EEPROM with avrdude

## Modifying the U3S EEPROM image

## Scripting it up!

The script below ties it all together.

```bash
#!/bin/sh

# make a copy of an existing "base" image
cp u3s.hex jt9-tmp.hex

# modify the transmission, message and frame fields
./u3s-eeprom-tool edit jt9-tmp.hex transmissions[0].mode "JT91"
./u3s-eeprom-tool edit jt9-tmp.hex transmissions[0].enabled true
./u3s-eeprom-tool edit jt9-tmp.hex transmissions[0].frequency 475200
./u3s-eeprom-tool edit jt9-tmp.hex transmissions[0].powerOrMessageIndex 0
./u3s-eeprom-tool edit jt9-tmp.hex message "$2"
./u3s-eeprom-tool edit jt9-tmp.hex frame.frame 2
./u3s-eeprom-tool edit jt9-tmp.hex frame.start $1

# cut off the last 768 bytes of the image (unused), to save programming time
cat jt9-tmp.hex | grep -v :200[123] > jt9-tmp-trunc.hex

# program it! (and reboot the U3S)
avrdude -c usbasp -p m328p -U eeprom:w:jt9-tmp-trunc.hex
```
