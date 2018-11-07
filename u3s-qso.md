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
