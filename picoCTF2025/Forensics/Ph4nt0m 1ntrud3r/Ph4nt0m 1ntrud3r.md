# Ph4nt0m 1ntrud3r
## Description:
A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag.
To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder!
Find the PCAP file here Network Traffic PCAP file and try to get the flag.

### Solution
1. First, sort the packets according to arrival timeline by using Wireshark.
2. Notice that there is data being base64-encoded. So, let's extract that and decode them using CyberChef.
```
cGljb0NURg== (picoCTF)
ezF0X3c0cw== ({1t_w4s)
bnRfdGg0dA== (nt_th4t)
XzM0c3lfdA== (_34sy_t)
YmhfNHJfZA== (bh_4r_d)
NGI1NzkwOQ== (4b57909)
fQ== (})
```
3. Concatenate everything and get we the flag.
4. picoCTF{1t_w4snt_th4t_34sy_tbh_4r_d4b57909}
