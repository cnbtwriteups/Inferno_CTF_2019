# [Merry Christmas](https://infernoctf.live/challenges#Merry%20Christmas)
Forensics,
200 Points

Author: mrT4ntr4

Writeup by: **archerrival**

## Description
>OSINT is the way :(

>Hint: What is mrT4ntr4's last name?

File Attached : [output.zip](https://infernoctf.live/files/cdd6e5cf98555210c4f3b7a9fa439fcd/output.zip?token=eyJ0ZWFtX2lkIjo3OCwidXNlcl9pZCI6MjEyLCJmaWxlX2lkIjoxMX0.XgcGmQ.orZ0uBC1zvL0aOHXIOQ5H7eyQ3g)

## Solution
When we try to extract the file, it prompts us for a password. Using the hint, we find that the last name of mrT4ntr4 is **Malhotra**.
Using that as our password, we are able to extract the file.

We receive a file called `flag.gif`. We try to open it, but we are unable to. Let's see what the file header says.
```
root@mark:/home# xxd -g 1 flag.gif | head
00000000: 41 41 41 41 41 61 53 04 69 00 e7 fb 00 32 00 00  AAAAAaS.i....2..
00000010: 38 00 02 3b 00 00 41 00 03 43 00 00 49 00 03 4d  8..;..A..C..I..M
00000020: 00 00 56 00 00 50 02 00 60 00 00 5c 04 00 6a 00  ..V..P..`..\..j.
00000030: 01 64 02 00 74 00 01 6f 03 00 78 01 00 7f 00 02  .d..t..o..x.....
00000040: 83 02 00 8a 00 02 8c 00 00 7c 08 02 66 0f 02 90  .........|..f...
00000050: 06 00 76 0e 06 88 0a 03 79 11 01 6b 15 09 81 10  ..v.....y..k....
00000060: 07 94 0d 03 85 13 02 71 19 05 8e 12 08 80 19 08  .......q........
00000070: 98 13 06 77 20 0c 94 1a 05 7d 21 08 86 1e 0d 8e  ...w ....}!.....
00000080: 1d 09 75 25 0c 9d 1a 0a ff 00 00 8b 23 09 a1 1e  ..u%........#...
00000090: 05 9b 22 0b 84 29 0c 8f 27 05 a5 23 08 98 27 0a  .."..)..'..#..'.
```

The file header does not seem to match that of a regular GIF file. The first 6 bytes of a GIF header must be `47 49 46 38 39 61`. Let's try
replacing the file header.

```
root@mark:/home# printf '\x47\x49\x46\x38\x39\x61' | dd of=flag.gif bs=1 seek=0 count=6 conv=notrunc
6+0 records in
6+0 records out
6 bytes copied, 0.001556 s, 3.9 kB/s
root@mark:/home# xxd -g 1 flag.gif | head
00000000: 47 49 46 38 39 61 53 04 69 00 e7 fb 00 32 00 00  GIF89aS.i....2..
00000010: 38 00 02 3b 00 00 41 00 03 43 00 00 49 00 03 4d  8..;..A..C..I..M
00000020: 00 00 56 00 00 50 02 00 60 00 00 5c 04 00 6a 00  ..V..P..`..\..j.
00000030: 01 64 02 00 74 00 01 6f 03 00 78 01 00 7f 00 02  .d..t..o..x.....
00000040: 83 02 00 8a 00 02 8c 00 00 7c 08 02 66 0f 02 90  .........|..f...
00000050: 06 00 76 0e 06 88 0a 03 79 11 01 6b 15 09 81 10  ..v.....y..k....
00000060: 07 94 0d 03 85 13 02 71 19 05 8e 12 08 80 19 08  .......q........
00000070: 98 13 06 77 20 0c 94 1a 05 7d 21 08 86 1e 0d 8e  ...w ....}!.....
00000080: 1d 09 75 25 0c 9d 1a 0a ff 00 00 8b 23 09 a1 1e  ..u%........#...
00000090: 05 9b 22 0b 84 29 0c 8f 27 05 a5 23 08 98 27 0a  .."..)..'..#..'.
```

If we try opening the file, we now see this lovely gif.

![](images/merrychristmas.gif)

`infernoCTF{M3RRy_ChR1stmAs}`
