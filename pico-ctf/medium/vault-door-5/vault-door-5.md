## Introduction

This is write-up for the challenge `picoCTF - vault-door-5`.
The goal is to obtain the flag from the Java program.
**Tools used:** Java, Code editor, Base64.

---

## Challenge Description

We are given the following Java program:
``` Java
import java.net.URLDecoder;
import java.util.*;

class VaultDoor5 {
    public static void main(String args[]) {
        VaultDoor5 vaultDoor = new VaultDoor5();
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

    // Minion #7781 used base 8 and base 16, but this is base 64, which is
    // like... eight times stronger, right? Riiigghtt? Well that's what my twin
    // brother Minion #2415 says, anyway.
    //
    // -Minion #2414
    public String base64Encode(byte[] input) {
        return Base64.getEncoder().encodeToString(input);
    }

    // URL encoding is meant for web pages, so any double agent spies who steal
    // our source code will think this is a web site or something, defintely not
    // vault door! Oh wait, should I have not said that in a source code
    // comment?
    //
    // -Minion #2415
    public String urlEncode(byte[] input) {
        StringBuffer buf = new StringBuffer();
        for (int i=0; i<input.length; i++) {
            buf.append(String.format("%%%2x", input[i]));
        }
        return buf.toString();
    }

    public boolean checkPassword(String password) {
        String urlEncoded = urlEncode(password.getBytes());
        String base64Encoded = base64Encode(urlEncoded.getBytes());
        String expected = "JTYzJTMwJTZlJTc2JTMzJTcyJTc0JTMxJTZlJTY3JTVm"
                        + "JTY2JTcyJTMwJTZkJTVmJTYyJTYxJTM1JTY1JTVmJTM2"
                        + "JTM0JTVmJTM4JTM0JTY2JTY0JTM1JTMwJTM5JTM1";
        return base64Encoded.equals(expected);
    }
}
```
## Analysis
1. The program expects input in the following format: `picoCTF{...}`.
2. The content inside the curly braces is passed to the `checkPassword` function.
3. The program processes the input as follows:
 - Input is converted into bytes.
 - Bytes are encrypted with base 64 encode.
4. The got encrypted password is compared with `expected` variable.

---

## Solution

Firstly we will decode the expected string:
``` bash
echo "JTYzJTMwJTZlJTc2JTMzJTcyJTc0JTMxJTZlJTY3JTVmJTY2JTcyJTMwJTZkJTVmJTYyJTYxJTM1JTY1JTVmJTM2JTM0JTVmJTM4JTM0JTY2JTY0JTM1JTMwJTM5JTM1" | base64 -d
%63%30%6e%76%33%72%74%31%6e%67%5f%66%72%30%6d%5f%62%61%35%65%5f%36%34%5f%38%34%66%64%35%30%39%35
```

Then we convert the got bytes into characters:
``` bash
>>> bytes = [ 0x63,0x30,0x6e,0x76,0x33,0x72,0x74,0x31,0x6e,0x67,0x5f,0x66,0x72,0x30,0x6d,0x5f,0x62,0x61,0x35,0x65,0x5f,0x36,0x34,0x5f,0x38,0x34,0x66,0x64,0x35,0x30,0x39,0x35 ]
>>> for i in bytes:
...     print(chr(i), end="")
c0nv3rt1ng_fr0m_ba5e_64_84fd5095
```

So now we can pass the original flag:
``` bash
java VaultDoor5
Enter vault password: picoCTF{c0nv3rt1ng_fr0m_ba5e_64_84fd5095}
Access granted.
```
