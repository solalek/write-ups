## Introduction

This is write-up for the challenge `picoCTF - vault-door-6`.
The goal is to obtain the flag from the Java program.
**Tools used:** Java, Code editor.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.util.*;

class VaultDoor6 {
    public static void main(String args[]) {
        VaultDoor6 vaultDoor = new VaultDoor6();
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

    // Dr. Evil gave me a book called Applied Cryptography by Bruce Schneier,
    // and I learned this really cool encryption system. This will be the
    // strongest vault door in Dr. Evil's entire evil volcano compound for sure!
    // Well, I didn't exactly read the *whole* book, but I'm sure there's
    // nothing important in the last 750 pages.
    //
    // -Minion #3091
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x66, 0x36, 0x30, 0x67, 0x6c, 0x64, 0x6c,
        };
        for (int i=0; i<32; i++) {
            if (((passBytes[i] ^ 0x55) - myBytes[i]) != 0) {
                return false;
            }
        }
        return true;
    }
}
```
## Analisys
1. The program expects input in the following format: `picoCTF{...}`.
2. The content in the curly braces is passed in the `checkPassword` function.
3. The content length must be equal 32.
4. The program processes the input as follows:
 - Input is converted into bytes.
 - The original bytes must be equal result of XOR(bytes from our input, 0x55).

---

## Solution

Since the program compare result of XOR operation and original bytes, we can obtain the needed bytes by reverse operation. For example 1001 XOR 0110 = 1111,  0110 XOR 1111 = 1001.
So we can use this function:
``` Java
public void getPass() {
    byte[] myBytes = {
        0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
        0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
        0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
        0xa , 0x66, 0x36, 0x30, 0x67, 0x6c, 0x64, 0x6c,
    };
    System.out.print("The password is: ");
    for (int i = 0; i < myBytes.length; i++) {
        System.out.print((char)(myBytes[i] ^ 0x55));
    }
    System.out.println();
}
```

So after using this function we will see the flag:
``` bash
java VaultDoor6
The password is: n0t_mUcH_h4rD3r_tH4n_x0r_3ce2919
Enter vault password: picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_3ce2919}
Access granted.
```


