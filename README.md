# Targeted deauthentication and WPA2 cracking

**NOTE:** For educational use to simulate wireless attacks to understand how they work and how to prevent them. Do **NOT** use for malicious intent.

Targeted packet sniffing with `airodump-ng` allows an adversary to deauthenticate a client from a server to capture the WPA2 handshake. This WPA2 handshake can be decrypted using tools such as `aircrack-ng`.

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
