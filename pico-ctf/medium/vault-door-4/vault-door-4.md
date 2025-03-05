## Introduction

This is write-up for the challenge `picoCTF - vault-door-4`.
The goal is to obtain the flag from the Java program.
**Tools used:** Java, Code editor.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.util.*;

class VaultDoor4 {
    public static void main(String args[]) {
        VaultDoor4 vaultDoor = new VaultDoor4();
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

    // I made myself dizzy converting all of these numbers into different bases,
    // so I just *know* that this vault will be impenetrable. This will make Dr.
    // Evil like me better than all of the other minions--especially Minion
    // #5620--I just know it!
    //
    //  .:::.   .:::.
    // :::::::.:::::::
    // :::::::::::::::
    // ':::::::::::::'
    //   ':::::::::'
    //     ':::::'
    //       ':'
    // -Minion #7781
    public boolean checkPassword(String password) {
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 070 , 0146,
            '4' , 'a' , '6' , 'c' , 'b' , 'f' , '3' , 'b' ,
        };
        for (int i=0; i<32; i++) {
            if (passBytes[i] != myBytes[i]) {
                return false;
            }
        }
        return true;
    }
}
```
## Analysis
1. The program expects the input in the following format: `picoCTF{...}`.
2. The content inside curly braces is passed in the `checkPassword` function.
3. The content is converted into bytes.
4. The converted bytes are compared with the original bytes.

---

## Solution

Since we have the original bytes, we can convert them bach to retrieve the original flag.
So we modify the `checkPassword` function:
``` Java
public boolean checkPassword(String password) {
    byte[] passBytes = password.getBytes();
    byte[] myBytes = {
        106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
        0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
        0142, 0131, 0164, 063 , 0163, 0137, 070 , 0146,
        '4' , 'a' , '6' , 'c' , 'b' , 'f' , '3' , 'b' ,
    };
    for (int i = 0; i < 32; i++) {
        System.out.print((char)myBytes[i]);
    }
    System.out.println();
    for (int i=0; i<32; i++) {
        if (passBytes[i] != myBytes[i]) {
            return false;
        }
    }
    return true;
}
```

And now we have the original flag:
``` bash
java VaultDoor4
Enter vault password: picoCTF{...}
jU5t_4_bUnCh_0f_bYt3s_8f4a6cbf3b
Access denied!
```

The flag: `picoCTF{jU5t_4_bUnCh_0f_bYt3s_8f4a6cbf3b}`.
