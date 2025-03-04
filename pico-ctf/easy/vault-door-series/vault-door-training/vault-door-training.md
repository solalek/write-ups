## Introduction

This is write-up for the challenge `picoCTF - vault-door-training`.
The goal is to obtain the flag from Java program.
**Tools used:** Java.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.util.*;

class VaultDoorTraining {
    public static void main(String args[]) {
        VaultDoorTraining vaultDoor = new VaultDoorTraining();
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

    // The password is below. Is it safe to put the password in the source code?
    // What if somebody stole our source code? Then they would know what our
    // password is. Hmm... I will think of some ways to improve the security
    // on the other doors.
    //
    // -Minion #9567
    public boolean checkPassword(String password) {
        return password.equals("w4rm1ng_Up_w1tH_jAv4_eec0716b713");
    }
}
```
## Analysis
1. The program expects input in the format `picoCTF{...}`.
2. The program verifies the password using the `checkPassword` function.

---

## Solution

If we scrutinize the function `checkPassword`, we will see that it just compare strings. One string is a flag we pass in the program and another string is a correct flag. So we can just copy this flag - `picoCTF{w4rm1ng_Up_w1tH_jAv4_eec0716b713}`.
