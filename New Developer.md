## [New Developer](https://infernoctf.live/challenges#New%20Developer)

OSINT, 50 Points

Author: nullpxl

Solved: -0x1C

>A friend of a friend of a friend who is known for leaking info was recently hired at a game company. 
>What can you find in their GitHub profile?
>https://github.com/iamthedeveloper123

## Solution:
Looking at the [link we are given](https://github.com/iamthedeveloper123), we can go through and check their repositories out. 

The first thing I did was check out [/bash2048](https://github.com/iamthedeveloper123/bash2048) as it was the first one on the repos list. 

Scrolling through, we see some *interesting* code for our loss condition of the game ([line 300-302](https://github.com/iamthedeveloper123/bash2048/blob/master/bash2048.sh#L300)) 
```
source ../dotfiles/.bashrc2
printf "\nYou have lost, try going to https://pastebin.com/$CODE for help!.  (And also for some secrets...) \033[0m\n"
exit 0
```

From this code, we can infer that there is information coming from the source file named [.bashrc2](https://github.com/iamthedeveloper123/dotfiles/blob/master/.bashrc2) located in [../dotfiles/](https://github.com/iamthedeveloper123/dotfiles)

Looking through the [.bashrc2](https://github.com/iamthedeveloper123/dotfiles/blob/master/.bashrc2) file for any information that might be getting exported or moved somehow to another file, we see that at [line 83](https://github.com/iamthedeveloper123/dotfiles/blob/master/.bashrc2#L83) we have the code

```
export CODE="trpNwEPT"
```

*very interesting*. 

Combining `https://pastebin.com/`and `trpNwEPT` yields us with a working link that contains the flag!

Link: https://pastebin.com/trpNwEPT

Mirror: https://web.archive.org/web/20191228071022/https://pastebin.com/raw/trpNwEPT
