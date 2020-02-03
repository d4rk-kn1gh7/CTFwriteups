**CHIP 8 /1**

The challenge was as follows:

```
I made this sweet web-based chip8 emulator.
The first 512 bytes of memory are protected!

http://167.172.165.153:60003/

Note: This challenge can bee solved using only chip8 instructions.
```

We were given a chip8 emulator with a basic interface as follows:
![Chip8-6](https://user-images.githubusercontent.com/54789221/73665060-7ba37400-46c6-11ea-9b87-277afe53d2af.png)

And the following list of commands:

![Chip8-1](https://user-images.githubusercontent.com/54789221/73662981-c02d1080-46c2-11ea-95a3-fb88b8fed94b.png)
![Chip8-2](https://user-images.githubusercontent.com/54789221/73662998-c7541e80-46c2-11ea-8300-29ef73757457.png)

Obviously, to read from the protected memory, we had to execute some code to be able to access that memory.
However, if we tried to set `I` to some memory within the protected range, we got the following error:
`SECURITY VIOLATION DETECTED: Can not set I outside of legal memory range.`

Interesting, so that meant we had to find an alternate way to get `I` inside that memory range.

After a bit of reading, one instruction looked interesting: `F X 29: Sets `I` to the location of the sprite for the character in VX`

I immediately realized that for different values stored in VX, `I` would get set to a different location, so `I` could potentially get set to a location inside the protected memory.

Trying the following input:
![Chip8-3](https://user-images.githubusercontent.com/54789221/73663015-cde29600-46c2-11ea-8c1f-7917574525da.png)

The value of `I` was set to zero! We managed to access the protected memory!

Now, all I had to do was change the value inside VX (in my case V1), until I got access to the required memory.

I also realised, after a bit of trial and error, that I could read information from this memory using the display command `D X Y N`,
with X and Y being co-ordinates, and N the height of the displayed output.

Displaying at V1=1:
![Chip8-4](https://user-images.githubusercontent.com/54789221/73663032-d509a400-46c2-11ea-8979-a2a42d1fb9d6.png)
However cool this output looked, it clearly wasnt the flag.

But, at V1=10, we hit a different output:
![Chip8-5](https://user-images.githubusercontent.com/54789221/73663044-d935c180-46c2-11ea-8dfb-2d50e733d40e.png)

Suddenly a thought struck my mind - what if the white pixels on the display were ones and the black zeros - thus leading to a binary output!
Trying that on the first line, we get `01001000`, which is the decimal equivalent of H! It's the beginning of the flag!

Continuing with our earlier method, we get the following binary output - 
```
01001000 01100001 01100011 01101011 01010100 01001101 01111011 01100001 00110101 00110101 01100101 01101101 00111000 01101100 01100101 01100100 01011111 01110011 00110000 01100011 01101011 01110011 01111101
```

Converting this to ASCII, here's our flag!
`HackTM{a55em8led_s0cks}`
