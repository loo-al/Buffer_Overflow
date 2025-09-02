# CECS 478 Buffer Overflow Lab Write Up: Alexander Loo

## 1st Attempt (not sucessful)
Usning the code from exploit 4 in smash the stack didn't yield any sucesful attempt.
![first_attempt](not_work.png)

## 2nd Attempt (not sucessful)
I referenced a guide found online.  And did all the necessary steps such as:

disabling the ASLR:
![disable](disable_aslr.png)

stepping into the vuln.c to find the return address and the adress of the buffer:
![stepping](step_in_gdb.png)

Although I didn't get root access, I was able to overflow the buffer:
![overflow](seg_fault.png)

I successfully removed the aslr randomization, but wasn't able to gain root access.
![root](no_root.png)

Ther version of Linux used was ubuntu 8.10 x86.

When calculating the offsets the ret address minus the buffer adress was 0xbff5c338 - 0xbff5c2d0 = 104.
Adding 104 + 4 = 108, should give access to the return address pointer and hijack it.

Adding 200-300 more to ebp will over over flow the buffer into arbitrary code execution in the stack which is in the exploit.c
That calculation was 0xbff5c338 + 200 in the hex calculator, yielding 0xbff5c400.  (this was adding 200)

On my furthest attempt my steps were as follows in the same terminal:
- Turn off aslr
- sudo ln -sf /bin/zsh bin/sh
- gcc -o vuln -g -z execstack -fno-stack-protector vuln.c
- change vuln permissions
```
sudo chown root vuln
sudo chmod 4755 vuln
```
- compile exploit.c
- make dummy bad file from exploit
- touch badfile
- run vuln
- whoami

I believe it was the offsets that were added into the adress that needed more refining and more trial and error to achieve root access.
This lab was appropriately the most difficult of the 3 we have done.  I've kept attempting to gain root access by ensuring the bad file was generated and not being empty. In the final exploit file there is at least the letter "w" in the badfile. Since Ubuntu 8.10 was so old I could not transfer files in and out of the OS with any easy.  Web browsers do not work on the OS since it is dated, so logging into github or any cloud was not possible.  


## Citations
smash the stack for fun and profit
https://www.youtube.com/watch?v=V9lMxx3iFWU
https://www.youtube.com/watch?v=hJ8IwyhqzD4
https://old-releases.ubuntu.com/releases/
https://www.rapidtables.com/convert/number/decimal-to-hex.html
https://www.calculator.net/hex-calculator.html?number1=bff5c338&c2op=%2B&number2=c8&calctype=op&x=Calculate
https://www.baeldung.com/linux/toggle-aslr-memory-randomization
https://aayushmalla56.medium.com/buffer-overflow-attack-dee62f8d6376
