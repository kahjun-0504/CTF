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
