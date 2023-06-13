# Targeted deauthentication and WPA2 cracking

## Introduction

**NOTE:** For educational use to simulate wireless attacks to understand how they work and how to prevent them. Do **NOT** use for malicious intent.

Targeted packet sniffing with `airodump-ng` allows an adversary to capture particular network traffic to discover available clients through sending probes and beacons and injecting deauthentication frames so the client attempts to reconnect, capturing the WPA2 handshake in the process. This WPA2 handshake can be decrypted using tools such as `aircrack-ng` to retrieve the PMK (Primary Master Key).

### Hardware used:
ALFA AWUS036 NHA
- Chipset: Atheros AR9271
- Frequency: 2.4GHz

### Tools used:
 - aircrack-ng
 - airodump-ng
 - airmon-ng

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

### Method 1 - Manually switching mode of operation.
```html
ifconfig wlan0 down
```
Disables your wireless interface.
```html
iwconfig wlan0 mode monitor
```
Sets your wireless interface's mode of operation to monitor.
```html
ifconfig wlan0 up
```
Enables your wireless interface.

### Method 2 - Using airmon-ng

```html
airmon-ng check kill
```
All conflicting processes are killed.
```html
sudo airmon-ng start wlan0
```
After running this command your mode of operation will be switched from managed to monitor enabling your wireless interface for capturing all frames in the air that are not directed for the interface. 
