## Introduction

This is write-up for challenge `picoCTF - vault-door-3`.
The goal is to obtain the flag from Java program.
**Tools used:** Java, Code editor.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
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

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");
    }
}
```
## Analysis
1. The program expects input in the following format: `picoCTF{...}`.
2. The content inside the curly braces is passed to the `checkPassword` function.
3. The program processes the input as follows:
 - The characters from 1 to 8 (indices 0 to 7) remain in the same order. 
 - The characters from 9 to 16 (indices 8 to 15) are reversed. 
 - The characters with numbers [17, 19, 21, 23, 25, 27, 29, 31] are reversed.
 - The characters with numbers [16, 18, 20, 22, 24, 26, 28, 30, 32] are reversed.
4. Then the program compares the got buffer with `jU5t_a_sna_3lpm12g94c_u_4_m7ra41`.

---

## Solution

Since the program just rearranges the order of characters, we can reverse this process to retrieve the original flag.
So we modify the program by adding the following code:
``` Java
        String s = new String(buffer);
        System.out.println(s); // added line.
        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");
```

Then we will pass this text `jU5t_a_sna_3lpm12g94c_u_4_m7ra41`.
``` bash
java VaultDoor3
Enter vault password: picoCTF{jU5t_a_sna_3lpm12g94c_u_4_m7ra41}
jU5t_a_s1mpl3_an4gr4m_4_u_c79a21
Access denied!
```

So now we have decrypted flag - `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}`.
``` bash
java VaultDoor3
Enter vault password: picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}
jU5t_a_sna_3lpm12g94c_u_4_m7ra41
Access granted.
```

