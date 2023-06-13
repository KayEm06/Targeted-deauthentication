# Targeted deauthentication and WPA2 cracking

**NOTE:** For educational use to simulate wireless attacks to understand how they work and how to prevent them. Do **NOT** use for malicious intent.

Targeted packet sniffing with `airodump-ng` allows an adversary to capture particular network traffic to discover available clients through sending probes and beacons and injecting deauthentication frames so the client attempts to reconnect, capturing the WPA2 handshake in the process. This WPA2 handshake can be decrypted using tools such as `aircrack-ng` to retrieve the PMK (Primary Master Key).

Hardware used:
ALFA AWUS036 NHA
- Chipset: Atheros AR9271
- Frequency: 2.4GHz

## Stage 1 - Installing the aircrack-ng suite

Install the dependencies for your operating system from https://github.com/aircrack-ng/aircrack-ng

To install for Ubuntu and Debian distros

```html
sudo apt-get update
sudo apt-get install -y aircrack-ng
```

To install for Red-hat distributions

```html
sudo dnf install -y aircrack-ng
```

## Stage 2 - Enabling monitor mode on your wireless adapter

