## Introduction

This is write-up for challenge `picoCTF - vault-door-1`.
The goal is to get flag from Java program.
**Tools used:** Java, Code editor, grep, cut, cat, tr, sed.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.util.*;

class VaultDoor1 {
    public static void main(String args[]) {
        VaultDoor1 vaultDoor = new VaultDoor1();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
	String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
	}
    }

    // I came up with a more secure way to check the password without putting
    // the password itself in the source code. I think this is going to be
    // UNHACKABLE!! I hope Dr. Evil agrees...
    //
    // -Minion #8728
    public boolean checkPassword(String password) {
        return password.length() == 32 &&
               password.charAt(0)  == 'd' &&
               password.charAt(29) == '3' &&
               password.charAt(4)  == 'r' &&
               password.charAt(2)  == '5' &&
               password.charAt(23) == 'r' &&
               password.charAt(3)  == 'c' &&
               password.charAt(17) == '4' &&
               password.charAt(1)  == '3' &&
               password.charAt(7)  == 'b' &&
               password.charAt(10) == '_' &&
               password.charAt(5)  == '4' &&
               password.charAt(9)  == '3' &&
               password.charAt(11) == 't' &&
               password.charAt(15) == 'c' &&
               password.charAt(8)  == 'l' &&
               password.charAt(12) == 'H' &&
               password.charAt(20) == 'c' &&
               password.charAt(14) == '_' &&
               password.charAt(6)  == 'm' &&
               password.charAt(24) == '5' &&
               password.charAt(18) == 'r' &&
               password.charAt(13) == '3' &&
               password.charAt(19) == '4' &&
               password.charAt(21) == 'T' &&
               password.charAt(16) == 'H' &&
               password.charAt(27) == 'f' &&
               password.charAt(30) == 'b' &&
               password.charAt(25) == '_' &&
               password.charAt(22) == '3' &&
               password.charAt(28) == '6' &&
               password.charAt(26) == 'f' &&
               password.charAt(31) == '0';
    }
}
```
## Analysis
1. Program expects input in the following format - `picoCTF{...}`.
2. It passes content in curly braces in the `checkPassword` function.
3. The `checkPassword` function compares every single character from our input with characters from correct flag
4. Comparison of characters are placed in disorder.

---

## Solution

Since the `checkPassword` function just compares our flag with correct flag, we can just copy it.
But, firstly, we need to place them in the correct order.
``` bash
cat VaultDoor1.java | grep "charAt" | cut -d "(" -f2 | tr ")" "." | sort -g | cut -d "'" -f2 | tr "\n" " " | sed "s/ //g"
```

Which will give us the following output: `d35cr4mbl3_tH3_cH4r4cT3r5_ff63b0`.

So now we can pass this flag.
``` bash
java VaultDoor1
Enter vault password: picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_ff63b0}
Access granted.
```
