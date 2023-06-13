# Targeted deauthentication and WPA2 cracking

## Introduction

**NOTE:** For educational use to simulate wireless attacks to understand how they work and how to prevent them. Do **NOT** use for malicious intent.

Targeted packet sniffing with `airodump-ng` enables an adversary to monitor network traffic, discovering available clients through sending probes and beacons. Deauthentication frames can then be injected using `aireplay-ng` to force the client to re-connect to the network, capturing the WPA2 handshake in the process. Using `aircrack-ng`, the EAPoL key frames captured in the four-way handshake can derive the PSK (Pre-shared key) with a dictionary attack which can be used to connect to the network, this opens a variety of attacks that can be performed such as ARP poisoining, bypassing HTTPS, bypassing HSTS, DNS spoofing, and injecting Javascript code.

### Extra information - 4 way handshake
<details>
<summary><b>Extra information - 4 way handshake</b></summary>
<br>
The Pre-Shared Key (PSK) is an authentication key that is used by clients to authorise themselves to a network. The PSK is
<ul><li>The first frame is an ANonce (Acknowledgement nunmber once) sent by the access point.</li>
<li>The second frame is an SNonce (Supplicant number once) which is protected by the Message Integrity Check (MIC) sent by the client. Once received by the access point, the access point generates a Pairwise Transient Key (PTK).</li>
<li>The third frame is a Robust Security Network (RSN) sent by the access point that includes information on the cipher suite, group cipher, and authentication method used.</li>
<li>Finally, the process is disestablished by the client.</li></ul>
</details>

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

```
sudo apt-get update
sudo apt-get install -y aircrack-ng
```

To install for Red-hat distributions

```
sudo dnf install -y aircrack-ng
```

## Stage 2 - Enabling monitor mode on your wireless adapter

### Method 1 - Manually switching mode of operation.
```
ifconfig wlan0 down
```
Disables your wireless interface.
```
iwconfig wlan0 mode monitor
```
Sets your wireless interface's mode of operation to monitor.
```
ifconfig wlan0 up
```
Enables your wireless interface.

### Method 2 - Using airmon-ng

```
airmon-ng check kill
```
All processes or services that control wireless interfaces will be killed to prevent interference.
```
sudo airmon-ng start wlan0
```
After running this command your mode of operation will be switched from managed to monitor enabling your wireless interface for capturing all frames in the air that are not directed for the interface.

## Stage 3 - Scanning for nearby networks

Beacons are periodically sent by access points to inform devices of their presence, to capture these beacon frames along with other information of nearby networks we use `airodump-ng`

```
airodump-ng wlan0mon
```
All nearby networks that operate on the 2.4GHz frequency range will be displayed on the terminal with additional information that may help an adversary identify a network with outdated encryption protocols or with a channel that is not congested.

```
CH  2 ][ Elapsed: 6 s ][ 2023-06-13 21:42                                                                      
                                                                                                                
BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID                                
                                                                                                                
**:**:**:**:**:**  -83        9        1    0   6  130   WPA2 CCMP   PSK  NOW6*****                            
**:**:**:**:**:**  -50       17        0    0   6  360   WPA2 CCMP   PSK  Redmi*****                           
28:C0:1B:20:05:6B  -35       10        0    0   1  130   WPA2 CCMP   PSK  EXT2-SKYFA72B                        
**:**:**:**:**:**  -54        8        0    0   1  195   WPA2 CCMP   PSK  TALKTALK7*****                       
**:**:**:**:**:**  -76        5        1    0   1  130   WPA2 CCMP   PSK  SKY*****                             
**:**:**:**:**:**  -48        4       34    0   1  130   WPA2 CCMP   PSK  SKY*****                             
**:**:**:**:**:**  -58        8        0    0   9  195   WPA2 CCMP   PSK  TALKTALK6*****                       
**:**:**:**:**:**  -70        4        0    0  11  130   OPN              LG_Speaker_Setup*****                
**:**:**:**:**:**  -58        4        0    0  11  195   WPA2 CCMP   PSK  TALKTALK7*****                       
```
- **BSSID**:   Basic Service Set Identifier, MAC address of the access point.
- **PWR**:     Power, signal strength of the access point.
- **Beacons**: The number of broadcast frames sent by the access point.
- **#Data**:   The number of data frames captured by the interface.
- **#/s**:     The number of data frames captured per second over 10 seconds.
- **CH**:      The channel number the access point is operating on.
- **MB**:      The maximum transmission speed supported by the access point.
- **ENC**:     The encryption algorithm used by the access point, OPN networks do not use an encryption algorithm.
- **CIPHER**:  The cipher used with the encryption algorithm.
- **AUTH**:    The authentication method used by the access point.
- **ESSID**:   Extended Service Set Identifier, name of the access point.

## Stage 4 - Discovering clients

Network clients should be scanned with the command below to produce a map of all clients connected to the network. The MAC address of connected clients is useful information in post connection attacks by spoofing your MAC address and redirecting network traffic to your machine.

```
sudo airmon-ng --bssid 28:C0:1B:20:05:6B -c 1 wlan0mon
```

```
CH  1 ][ Elapsed: 12 s ][ 2023-06-13 22:08                                                                     
                                                                                                               
BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID                            
                                                                                                               
28:C0:1B:20:05:6B  -30 100      112       36   17   1  130   WPA2 CCMP   PSK  EXT2-SKYFA72B                    
                                                                                                               
BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes                              
                                                                                                               
28:C0:1B:20:05:6B DC:80:84:67:84:4A  -36    6e-24      0      162                                              
```
