# zizzania

zizzania sniffs wireless traffic listening for WPA handshakes and dumping only those frames suitable to be decrypted (one beacon + EAPOL frames + data). In order to speed up the process, zizzania sends IEEE 802.11 DeAuth frames to the stations whose handshake is needed, properly handling retransmissions and reassociations and trying to limit the number of DeAuth frames sent to each station.

![Screenshot](https://i.imgur.com/zGxPSTE.png)

## Examples

Put the network interface in RFMON mode on channel 6 and save the traffic gathered from the stations associated to a specific access point excluding those whose MAC address starts with `00:11:22`:

```
zizzania -i wlan0 -c 6 -b AA:BB:CC:DD:EE:FF -x 00:11:22:33:44:55/ff:ff:ff:00:00:00 -w out.pcap
```

---

Passively analyze the traffic generated by any station on the current channel assuming that the network interface is already RFMON mode:

```
zizzania -i wlan0 -n
```

---

Strip unnecessary frames from a pcap file (excluding altogether the traffic generated by one particular station) considering an handshake complete after just the first two messages (which should be enough for unicast traffic decryption):

```
zizzania -r in.pcap -x 00:11:22:33:44:55 -w out.pcap
```

## Setup

### Dependencies

For Debian-based systems:

```
sudo apt-get install libpcap-dev
```

For macOS systems (Homebrew):

```
brew install libpcap wget
```

### Building

```
make -f config.Makefile
make -j "$(nproc)"
```

### Installation

The installation process is not mandatory, zizzania can be run from the `src` directory. Just in case:

```
make install
make uninstall
```

## macOS support

Channel switching must be performed manually:

```
ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/local/bin/airport
sudo airport --disassociate
sudo airport --channel=<channel>
```
