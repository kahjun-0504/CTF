# Bitlocker-1
## Description:
Jacky is not very knowledgable about the best security passwords and used a simple password to encrypt their BitLocker drive. See if you can break through the encryption!
Download the disk image here

### Solution
1. First, identify the file format using **file bitlocker-1.dd**
2. We get this, `bitlocker-1.dd: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "-FVE-FS-", sectors/cluster 8, reserved sectors 0, Media descriptor 0xf8, sectors/track 63, heads 255, hidden sectors 124499968, FAT (32 bit), sectors/FAT 8160, serial number 0, unlabeled; NTFS, sectors/track 63, physical drive 0x1fe0, $MFT start cluster 393217, serial number 02020454d414e204f, checksum 0x41462020`, indicating BitLocker.
3. From the description and hint given, the BitLocker drive is encrypted using simple password and we need to crack the hash.
4. To extract the BitLocker recovery hash, **sudo bitlocker2john -i ./bitlocker-1.dd > hash.txt**
5. The hash.txt file contains all the hashes.
```
Encrypted device ./bitlocker-1.dd opened, size 100MB
Salt: 2b71884a0ef66f0b9de049a82a39d15b
RP Nonce: 00be8a46ead6da0106000000
RP MAC: a28f1a60db3e3fe4049a821c3aea5e4b
RP VMK: a1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58

UP Nonce: d04d9c58eed6da010a000000
UP MAC: 68156e51e53f0a01c076a32ba2b2999a
UP VMK: fffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d


User Password hash:
$bitlocker$0$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d
Hash type: User Password with MAC verification (slower solution, no false positives)
$bitlocker$1$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d
Hash type: Recovery Password fast attack
$bitlocker$2$16$2b71884a0ef66f0b9de049a82a39d15b$1048576$12$00be8a46ead6da0106000000$60$a28f1a60db3e3fe4049a821c3aea5e4ba1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58
Hash type: Recovery Password with MAC verification (slower solution, no false positives)
$bitlocker$3$16$2b71884a0ef66f0b9de049a82a39d15b$1048576$12$00be8a46ead6da0106000000$60$a28f1a60db3e3fe4049a821c3aea5e4ba1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58

```
6. Crack the hashes using hashcat, **hashcat -m 22100 hash.txt rockyou.txt**
7. Show the cracked hash, **hashcat -m 22100 hash.txt rockyou.txt --show**
8. We successfully cracked the hash and get the password, jacqueline.
9. To open BitLocker-encrypted .dd file in Linux, install dislocker.
10. **sudo apt update && sudo apt install dislocker**
11. Create a mount point.
 ```
mkdir -p /mnt/bitlocker
mkdir -p /mnt/bitlocker_decrypted
 ```
12. Unlock the BitLocker image using password, **sudo dislocker -r -V bitlocker-1.dd -u -- /mnt/bitlocker** and password will be prompted.
13. Mount the decrypted partition, **sudo mount -o loop /mnt/bitlocker/dislocker-file /mnt/bitlocker_decrypted**
14. Navigating to the mount point, we get the flag: picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}
15. Unmount after use.
```
sudo umount /mnt/bitlocker_decrypted
sudo umount /mnt/bitlocker
```


