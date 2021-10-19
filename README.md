## VeraCrypt Short Password Mod

Here you'll find a patched executable for the Windows version of VeraCrypt that allows short passwords even when using a smaller PIM (personal iteration multiplier).


## Usage guide

#### Create a regular volume
* Download and install version 1.24-Update7 from the [official](https://www.veracrypt.fr/) source.
* Create your encrypted volume/system as you normally would. Just use the default (high) PIM.
* Store away the created Rescue Disc that uses the default PIM as well.

#### Use VeraCrypt Short Password Mod
* Download [VeraCrypt-v1.24-Update7-short-pw-mod.exe](https://github.com/Unit520/VeraCrypt-Short-Password-Mod/releases/download/v1.24-Update7/VeraCrypt-v1.24-Update7-short-pw-mod.exe).
* Make sure the VeraCrypt background task isn't running, otherwise it will just open a window from the already running process.
* Launch the patched version from any directory and change your password (System&#8594;Change PW or Volume Tools&#8594;Change PW).
* Select a lower PIM (PIM = 1 is similar to TrueCrypt's behavior).
* You will still get a short password warning, but it lets you proceed after that.
* Optionally, the PIM can be stored inside the bootloader in case of an encrypted system, or using the favorite volumes feature for regular volumes. This way you don't have to enter your custom PIM every time.

Enjoy fast mounting times while not having to use a 20+ character long password!


### What are the security implications of low PIM and short password?
It depends on your threat model and against whom or what you are trying to protect your data.
It's always a question of how much effort somebody is willing to put in and how important your data is to you, and what scenarios that go against you you can tolerate.

For example, I use system encryption on my notebook in case it gets lost or stolen.
Now, a thief most likely isn't interested in the data on the device, and so isn't the person that might find your lost notebook. However, curiosity gets the best of people, so they might be inclined to peek around your stuff. In this scenario, any encryption whatsoever with a password better than *123* or *password* will be more than enough. And even if any form of brute force password guessing is attempted, who wants to spend more than a couple of hours or days on that?

Other scenarios, like three letter agencies or malicious people just use keyloggers or similar anyway, or just [do what they've always done](https://xkcd.com/538/).


### Can I trust this?

The actual changes are very minor, two modified assembly instructions and a remark in the Help&#8594;About window to identify the patched version. I encourage you to take a look yourself and compare it to the original VeraCrypt.exe.\
For example, on a standard bash shell you could do:
```bash
diff <(xxd -u VeraCrypt-v1.24-Update7-original.exe) <(xxd -u VeraCrypt-v1.24-Update7-short-pw-mod.exe)
```

which leads to the following output:
```diff
23183c23183
< 0005a8e0: 0DFF 1521 120C 0048 8935 EA86 1A00 8BC3  ...!...H.5......
---
> 0005a8e0: 0DFF 1521 120C 0048 8935 EA86 1A00 FFC0  ...!...H.5......
31526c31526
< 0007b250: FA14 735A 4585 D274 2048 8D05 0844 0B00  ..sZE..t H...D..
---
> 0007b250: FA01 735A 4585 D274 2048 8D05 0844 0B00  ..sZE..t H...D..
76471,76475c76471,76475
< 0012ab60: 5200 6500 6C00 6500 6100 7300 6500 6400  R.e.l.e.a.s.e.d.
< 0012ab70: 2000 6200 7900 2000 4900 4400 5200 4900   .b.y. .I.D.R.I.
< 0012ab80: 5800 2000 6F00 6E00 2000 4100 7500 6700  X. .o.n. .A.u.g.
< 0012ab90: 7500 7300 7400 2000 3700 2C00 2000 3200  u.s.t. .7.,. .2.
< 0012aba0: 3000 3200 3000 0000 5600 6500 7200 6100  0.2.0...V.e.r.a.
---
> 0012ab60: 5300 6800 6F00 7200 7400 2000 5000 6100  S.h.o.r.t. .P.a.
> 0012ab70: 7300 7300 7700 6F00 7200 6400 2000 4D00  s.s.w.o.r.d. .M.
> 0012ab80: 6F00 6400 2000 6200 7900 2000 5500 6E00  o.d. .b.y. .U.n.
> 0012ab90: 6900 7400 2000 3500 3200 3000 2000 2000  i.t. .5.2.0. . .
> 0012aba0: 2000 2000 2000 0000 5600 6500 7200 6100   . . ...V.e.r.a.
```
