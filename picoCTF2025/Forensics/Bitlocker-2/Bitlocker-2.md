# Bitlocker-2
## Description:
Jacky has learnt about the importance of strong passwords and made sure to encrypt the BitLocker drive with a very long and complex password. We managed to capture the RAM while this drive was opened however. See if you can break through the encryption!
Download the disk image here and the RAM dump here

## Hints:
Try using a volatility plugin

### Solution
1. For this challenge, I try to search for suitable plugin for volatility and I came across this bitlocker plugin in Volatility 2: _https://github.com/breppo/Volatility-BitLocker_. This plugin is not available in Volatility 3, so we will be using Volatility 2 for this challenge.
2. The Volatility framework can be download here: _https://github.com/volatilityfoundation/volatility_. I download the ZIP file instead of git clone it due to some errors.
3. Next, Volatility 2 needs python 2 and pip 2 installed as describe here: _https://www.hackthebox.com/blog/memory-forensics-volatility-write-up_. To install python 2,
```
sudo apt install python2
```
4. To install pip 2, use the following script from this link: _https://gist.github.com/anir0y/a20246e26dcb2ebf1b44a0e1d989f5d1_
```
#!/bin/bash

echo "Setting up pip2"
mkdir scripts && cd scripts
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
echo "Enter Sudo password is asked"
sleep 2
sudo python2 get-pip.py
pip2 install --upgrade setuptools
sudo apt-get install python-dev -y 
clear
echo "----------"
echo "DONE!!!"
echo "----------"
```
5. Next, create python virtual environment.
```
python3 -m venv myenv
```
6. Activate the virtual environment.
```
source myenv/bin/activate
```
7. Install pyrcryptodome and distorm3
```
pip2 install distorm3 pyrcryptodome
```
8. To use Volatility 2, we have to determine the memory dump image profile.
```
python2 vol.py -f /home/kali/Downloads/memdump.mem kdbgscan
```
9. Then, specify the profile determined using --profile flag.
```
python2 vol.py -f /home/kali/Downloads/memdump.mem --profile=Win10x64_19041 -h
```
10. Notice that the bitlocker plugin does not come install with Volatility 2, so we have to copy the bitlocker.py file into the volatily plugin path as describe here: _https://github.com/breppo/Volatility-BitLocker_
11. Else, we may specify the path of the plugin using --plugins flag if it is placed in different location.
```
python2 vol.py --plugins=/home/kali/Downloads/plugins --info | grep bitlocker
```
12. Then, dump the potential found FVEKs into a path you defined using --dislocker flag.
```
python2 vol.py -f /home/kali/Downloads/memdump.mem --profile=Win10x64_19041 --plugins=/home/kali/Downloads/plugins bitlocker --dislocker /home/kali/Downloads/dump
```
13. The output should look like this:
```
[FVEK] Address : 0x8087865bead0
[FVEK] Cipher  : AES-XTS 128 bit (Win 10+)
[FVEK] FVEK: 4f79d4a00d5e9b25965b89581a6a599c
[DISL] FVEK for Dislocker dumped to file: /home/kali/Downloads/dump/0x8087865bead0-Dislocker.fvek

[FVEK] Address : 0x40d857c90
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: d40582190eb6f067691120bbbe55e511
[DISL] FVEK for Dislocker dumped to file: /home/kali/Downloads/dump/0x40d857c90-Dislocker.fvek
```
14. The first returned should be the one as the plugin goes from the current Windows versions to the oldest.
15. Next, create mount points.
```
sudo mkdir -p /mnt/bitlocker
sudo mkdir -p /mnt/bitlocker_decrypted
```
16. Unlock the BitLocker image using FVEK. Specify the path of the FVEK obtained in Step 12 and 13 using --fvek flag.
```
sudo dislocker-fuse -V bitlocker-2.dd --fvek=/home/kali/Downloads/dump/0x8087865bead0-Dislocker.fvek -- /mnt/bitlocker
```
17. Mount the decrypted partition.
```
sudo mount -t ntfs-3g /mnt/bitlocker/dislocker-file /mnt/bitlocker_decrypted
```
18. _-- /mnt/bitlocker_ defines the output location where Dislocker will create a decrypted virtual drive (dislocker-file).
19. Get the flag by going to the mount point /mnt/bitlocker_decrypted.
20. 
