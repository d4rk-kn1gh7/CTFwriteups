**CHIP 8 /2**

The challenge was as follows:

```
I made this sweet web-based chip8 emulator.
The last 512 bytes of memory are protected!

http://167.172.165.153:60003/

Note: This challenge can bee solved using only chip8 instructions.
```

We were given a chip8 emulator with a basic interface as follows:

![Chip8-6](https://user-images.githubusercontent.com/54789221/73665319-e6ed4600-46c6-11ea-8a6a-156b287c9f40.png)

And the following list of commands:

![Chip8-1](https://user-images.githubusercontent.com/54789221/73662981-c02d1080-46c2-11ea-95a3-fb88b8fed94b.png)
![Chip8-2](https://user-images.githubusercontent.com/54789221/73662998-c7541e80-46c2-11ea-8300-29ef73757457.png)

This challenge involves reading from the last few bytes of memory, which are again protected.

Messing around with the commands, it was pretty evident that there was no way to set `I` to anywhere in the last few memory addresses.

So what was the next possible thing? Code execution within that memory range. 
The command `2 NNN`, which calls a subroutine at address 'NNN', was the command to accomplish this.

Using the command `2 E00`, which calls the function at address E00 (4096 - 511 bytes, the first address in our forbidden memory), we get a weird error:

![Chip8-7](https://user-images.githubusercontent.com/54789221/73670541-6aab3080-46cf-11ea-938a-ccbe7ca96601.png)

`Invalid instruction: 5F61`

It's pretty clear that '5F' and '61' refer to the ASCII characters '_' and 'a' respectively, but what does this mean? 
Could we be executing the hex values of the characters in the flag?

Let's take another look. Executing step-by-step this time:

After a few steps we see something interesting:

![Chip8-8](https://user-images.githubusercontent.com/54789221/73670950-1e142500-46d0-11ea-997a-16856604b73a.png)

`Last instruction 'AA48'`

Well, what do we have here? '48' is the hex value of 'H', which is obviously the start of our flag!

From here on, all we have to do is gather the hex bytes from subsequent instructions, 
and skip forward an instruction when we hit an invalid instruction (continue execution from the next instruction).

Thus, we get the following collection of hex bytes:
```
48 61 63 6b 54 4d 7b 6a 75 64 36 65 5f 6a 75 72 79 5f 61 6e 64 5f 33 78 33 63 75 74 31 6f 6e 7d
```

And there's our flag! `HackTM{jud6e_jury_and_3x3cut1on}`
