## Introduction

This is write-up for the challenge `picoCTF - vault-door-7`.
The goal is to obtain the flag from the Java program.
**Tools used:** Java, Code editor, Sed, Tr, Fold, Python.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;

class VaultDoor7 {
    public static void main(String args[]) {
        VaultDoor7 vaultDoor = new VaultDoor7();
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

    // Each character can be represented as a byte value using its
    // ASCII encoding. Each byte contains 8 bits, and an int contains
    // 32 bits, so we can "pack" 4 bytes into a single int. Here's an
    // example: if the hex string is "01ab", then those can be
    // represented as the bytes {0x30, 0x31, 0x61, 0x62}. When those
    // bytes are represented as binary, they are:
    //
    // 0x30: 00110000
    // 0x31: 00110001
    // 0x61: 01100001
    // 0x62: 01100010
    //
    // If we put those 4 binary numbers end to end, we end up with 32
    // bits that can be interpreted as an int.
    //
    // 00110000001100010110000101100010 -> 808542562
    //
    // Since 4 chars can be represented as 1 int, the 32 character password can
    // be represented as an array of 8 ints.
    //
    // - Minion #7816
    public int[] passwordToIntArray(String hex) {
        int[] x = new int[8];
        byte[] hexBytes = hex.getBytes();
        for (int i=0; i<8; i++) {
            x[i] = hexBytes[i*4]   << 24
                 | hexBytes[i*4+1] << 16
                 | hexBytes[i*4+2] << 8
                 | hexBytes[i*4+3];
        }
        return x;
    }

    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        int[] x = passwordToIntArray(password);
        return x[0] == 1096770097
            && x[1] == 1952395366
            && x[2] == 1600270708
            && x[3] == 1601398833
            && x[4] == 1716808014
            && x[5] == 1734293296
            && x[6] == 842413104
            && x[7] == 1684157793;
    }
}
```
## Analisys
1. The program expects the following format: `picoCTF{...}`.
2. The content from the curly braces is passed in the `checkPassword` function.
3. The content length must be equal 32.
4. The principle of work is described in the got Java program.

---

## Solution

Firstly we will copy numbers and convert them into binary numbers using python:
``` bash
>>> numbers = [ 1096770097, 1952395366, 1600270708, 1601398833, 1716808014, 1734293296, 842413104, 1684157793 ]
>>> for i in numbers:
...     print(bin(i))
...
0b1000001010111110110001000110001
0b1110100010111110011000001100110
0b1011111011000100011000101110100
0b1011111011100110110100000110001
0b1100110010101000110100101001110
0b1100111010111110011011100110000
0b110010001101100011010000110000
0b1100100011000100011010101100001
```

Next we can notice that there are only 31 characters by using `wc`:
``` bash
echo "1000001010111110110001000110001" | wc -c
32
```

Since `echo` adds `\n` in the end by default, there 31 bits and `\n` in the end, so total is 32. 
But one line has even 30 bits, in this case we will manually add 1 zero.
Next we will add 0 in the beginning of every single line and divide this lines by 4 bits, so we will use the following commads:
``` bash
echo "0b1000001010111110110001000110001
  0b1110100010111110011000001100110
  0b1011111011000100011000101110100
  0b1011111011100110110100000110001
  0b1100110010101000110100101001110
  0b1100111010111110011011100110000
  0b0110010001101100011010000110000
  0b1100100011000100011010101100001" | sed "s/0b/0b0/g" | tr "\n" "," | sed "s/0b//g" | sed "s/,//g" | fold -w 8 | tr "\n" "," | sed "s/^/0b/g" | sed "s/,/,0b/g"
0b01000001,0b01011111,0b01100010,0b00110001,0b01110100,0b01011111,0b00110000,0b01100110,0b01011111,0b01100010,0b00110001,0b01110100,0b01011111,0b01110011,0b01101000,0b00110001,0b01100110,0b01010100,0b01101001,0b01001110,0b01100111,0b01011111,0b00110111,0b00110000,0b00110010,0b00110110,0b00110100,0b00110000,0b01100100,0b01100010,0b00110101,0b01100001
```

Then we will use python to convert this numbers into characters:
``` bash
>>> bytes = [ 0b01000001,0b01011111,0b01100010,0b00110001,0b01110100,0b01011111,0b00110000,0b01100110,0b01011111,0b0\
1100010,0b00110001,0b01110100,0b01011111,0b01110011,0b01101000,0b00110001,0b01100110,0b01010100,0b01101001,0b0100111\
0,0b01100111,0b01011111,0b00110111,0b00110000,0b00110010,0b00110110,0b00110100,0b00110000,0b01100100,0b01100010,0b00\
110101,0b01100001 ]
>>> for i in bytes:
...     print(chr(i), end="")
... print("\n")
...
A_b1t_0f_b1t_sh1fTiNg_702640db5a
```

Finally, we will pass the original flag:
``` bash
java VaultDoor7
Enter vault password: picoCTF{A_b1t_0f_b1t_sh1fTiNg_702640db5a}
Access granted.
```
