## [New Developer](https://infernoctf.live/challenges#Where%20did%20he%20GO?)

Reversing, 50 Points

Author: Unknown

Writeup By: **-0x1C**

>Get Set GO...      
>Flag Format: infernoCTF{#string_here}

File Attached: [test.go](https://web.archive.org/web/20191228072229/https://infernoctf.live/files/8b9fd7e9c1ef8ca67dd8165f314af7c8/test.go?token=eyJ0ZWFtX2lkIjo3OCwidXNlcl9pZCI6MjU2LCJmaWxlX2lkIjoxMH0.XgcCBg.LUsVJllpOsczkcHoWrQmh8WunYM)

## Solution:
Opening the .go file in a text editor will show us the source code of the program at hand.

```go
package main

import (
	"fmt"
	"strings"
	"os"
	"bufio";
)

func jai_ram_ji_ki(s string) string {
	chars := []rune(s)
	for i, j := 0, len(chars)-1; i < j; i, j = i+1, j-1 {
		chars[i], chars[j] = chars[j], chars[i]
	}
	return string(chars)
}

func EncryptDecrypt(input, key string) (output string) {
        for i := 0; i < len(input); i++ {
                output += string(input[i] ^ key[i % len(key)])
        }

        return output
}

func mandir_wahi_banega(s string) string {
	words := strings.Fields(s)
	for i, j := 0, len(words)-1; i < j; i, j = i+1, j-1 {
		words[i], words[j] = jai_ram_ji_ki(words[j]), jai_ram_ji_ki(words[i])
	}
	return strings.Join(words, "_")
}

func main() {
	fmt.Print("Enter Password: ");
        user_input,_,err := bufio.NewReader(os.Stdin).ReadLine();
        if err != nil {
            fmt.Println("Something is wrong with your computer, ",err);
	}
	ency := string([]byte{33,33,116,65,51,114,71,95,115,49,95,103,110,49,77,77,97,82,103,48,114,80,95,48,103})
	if jai_ram_ji_ki(mandir_wahi_banega(string(user_input))) == ency {
        fmt.Println("You Cracked it, A Hero is born");
    	} else {
        fmt.Println("Don't Worry, Relax, Chill and Try harder");
	}
}
```

Giving the main() function a look, we can see that the program will read in an input and check if `jai_ram_ji_ki(mandir_wahi_banega(string(user_input))) == ency`

Although this might seem complicated, we can see that all we need to do to get the solution is have a string that evaluates to the value of the string `ency`.

We are given the string `ency` in its char bytes, `[]byte{33,33,116,65,51,114,71,95,115,49,95,103,110,49,77,77,97,82,103,48,114,80,95,48,103}`, which we can easily reverse back to plaintext by converting them from hex to ascii characters, then reversing the string.

A basic script to grab the flag from the bytes can be made as such

```python
original_string = [33,33,116,65,51,114,71,95,115,49,95,103,110,49,77,77,97,82,103,48,114,80,95,48,103]
final_string = ""

# Iterate through each character in original_string
for current_character in original_string:
  final_string += chr(current_character) # Add each character into final_string

print(final_string[::-1]) # Reverse the final string
```

The output of this script will be the flag!

Flag: `infernoCTF{g0_Pr0gRaMM1ng_1s_Gr3At!!}`
