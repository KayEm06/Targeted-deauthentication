# Targeted deauthentication and WPA2 cracking

**NOTE:** For educational use to simulate wireless attacks to understand how they work and how to prevent them. Do **NOT** use for malicious intent.

Targeted packet sniffing with `airodump-ng` allows an adversary to deauthenticate a client from a server to capture the WPA2 handshake. This WPA2 handshake can be decrypted using tools such as `aircrack-ng`.

## Stage 1 - Installing the aircrack-ng suite

```html
∼$ sudo apt update
∼$ sudo apt install aircrack-ng
```
