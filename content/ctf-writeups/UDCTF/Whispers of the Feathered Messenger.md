## Whispers of the Feathered Messenger
- Point : 100
- Category : forensics
- Description : In a world where secrets flutter through the air, the bluehen carries a hidden message. A message that has been salted.... however its still a message... maybe the bluehen ignores the salt. This image holds more than meets the eye.
- shasum: e717eefe9b41212b017152756b0e640f9a4f3763 bird.jpeg
- @PotateL



## Approach

1.  We get a bird.jpeg file . Lets check what type of file is this using file command

```
┌──(kali㉿kali)-[~/CTF/UDCTF/forensic/Whispers of the Feathered Messenger]
└─$ file bird.jpeg               
bird.jpeg: JPEG image data, JFIF standard 1.01, aspect ratio, density 72x72, segment length 16, comment: "UGFzc3dvcmQ6IDVCNEA3cTchckVc", baseline, precision 8, 1080x1350, components 3
```

2.  It got comment?? weird string . let try to decipher it using cyberchef.io

- When we add From Base64 recipe we got this string
```
Password: 5B4@7q7!rE\
```
Yay we got a password


3.  Maybe we can use this password for steghide lets try it. I will paste the password to the passphrase of the steghide
```
┌──(kali㉿kali)-[~/CTF/UDCTF/forensic/Whispers of the Feathered Messenger]
└─$ steghide extract -sf bird.jpeg                                   
Enter passphrase: 
wrote extracted data to "encrypted_flag.bin".

```
Yay we got it. We got new file called `encypted_flag.bin`

4. Let see this what type of file this is using binwalk
```
┌──(kali㉿kali)-[~/CTF/UDCTF/forensic/Whispers of the Feathered Messenger]
└─$ binwalk encrypted_flag.bin 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             OpenSSL encryption, salted, salt: 0x7788534E4EBCE329
```
I see it is an OpenSSL file. 

5. Lets try to decrypt using openssl by using the most common encryption in openssl which is aes-256-cbc
```
┌──(kali㉿kali)-[~/CTF/UDCTF/forensic/Whispers of the Feathered Messenger]
└─$ openssl enc -d -aes-256-cbc -in encrypted_flag.bin -out decrypted_flag.txt -salt -pass pass:'5B4@7q7!rE\' 
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.                                     
```

6.  Let's see our output file .
```
┌──(kali㉿kali)-[~/CTF/UDCTF/forensic/Whispers of the Feathered Messenger]
└─$ cat decrypted_flag.txt 
UDCTF{m0AybE_YoR3$!_a_f0recnicsEs_3xpEr^t}                                         
```
Yay and we got the flag : `UDCTF{m0AybE_YoR3$!_a_f0recnicsEs_3xpEr^t}`