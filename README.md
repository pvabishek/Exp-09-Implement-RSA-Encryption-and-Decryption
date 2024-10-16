# Exp-09-Implement-RSA-Encryption-and-Decryption
## AIM:
To implement the RSA algorithm to securely encrypt and decrypt messages using public and private keys.
## ALGORITHM:
### Step 1: 
Get two distinct prime numbers p and q from the user or predefine them. 
### Step 2: 
Calculate the modulus n by multiplying p and q: n=p*q.
### Step 3:
Calculate Euler’s Totient function φ(n): φ(n) = (p-1) * (q-1)
### Step 4:
Choose a public exponent e such that: 1 < e < φ(n) = 1 and gcd(e, φ(n)) = 1(ensure e is coprime with φ(n))
### Step 5:
Calculate the private key d as the modular inverse of e modulo φ(n): d . e ≡ 1(mod φ(n))
### Step 6:
Formulate the public key as the pair (e, n) and the private key as the pair (d, n)
### Step 7:
Ask the user for an alphabetic message to encrypt.
### Step 8:
Convert each character in the message to its ASCII value.
### Step 9:
Encrypt each ASCII value using the formula: C=M^e mod n (where M is the ASCII value and C is the ciphertext)
### Step 10:
Decrypt each ciphertext using the formula: M=C^d mod n (where C is the ciphertext and M is the decrypted ASCII value).
### Step 11:
Convert the decrypted ASCII values back to characters to retrieve the original message. 
### Step 12:
Print the encrypted message and the decrypted message.
### Step 13:
End the program.

## PROGRAM:
```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <ctype.h>
#include <stdlib.h>

// Function to calculate GCD using the Euclidean algorithm
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// Function to calculate (base^exp) % mod using modular exponentiation
long long mod_exp(long long base, long long exp, long long mod) {
    long long result = 1;
    while (exp > 0) {
        if (exp % 2 == 1)
            result = (result * base) % mod;
        base = (base * base) % mod;
        exp = exp / 2;
    }
    return result;
}

// Function to calculate the modular inverse of e mod phi using the extended Euclidean algorithm
int mod_inverse(int e, int phi) {
    int t = 0, newt = 1;
    int r = phi, newr = e;
    while (newr != 0) {
        int quotient = r / newr;
        int temp = t;
        t = newt;
        newt = temp - quotient * newt;
        temp = r;
        r = newr;
        newr = temp - quotient * newr;
    }
    if (r > 1) return -1; // e is not invertible
    if (t < 0) t = t + phi;
    return t;
}

int main() {
    // Step 1: Initialize prime numbers p and q (use larger primes for real-world applications)
    int p = 61;
    int q = 53;
    
    // Step 2: Compute n = p * q and phi = (p-1) * (q-1)
    int n = p * q;
    int phi = (p - 1) * (q - 1);

    // Step 3: Choose an encryption key e such that 1 < e < phi and gcd(e, phi) = 1
    int e = 17; // A commonly used public exponent
    if (gcd(e, phi) != 1) {
        printf("e and phi(n) are not coprime!\n");
        return -1;
    }

    // Step 4: Compute the decryption key d, the modular inverse of e mod phi
    int d = mod_inverse(e, phi);
    if (d == -1) {
        printf("No modular inverse found for e!\n");
        return -1;
    }

    // Step 5: Display the public and private keys
    printf("Public Key: (e = %d, n = %d)\n", e, n);
    printf("Private Key: (d = %d, n = %d)\n", d, n);

    // Step 6: Get the message from the user
    char message[100];
    printf("Enter a message to encrypt (alphabetic characters only): ");
    fgets(message, sizeof(message), stdin);
    int len = strlen(message);
    if (message[len - 1] == '\n') message[len - 1] = '\0'; // Remove newline character

    // Step 7: Encrypt the message
    printf("\nEncrypted Message:\n");
    long long encrypted[100];
    for (int i = 0; i < len; i++) {
        int m = (int)message[i];  // Convert the character to its ASCII value
        encrypted[i] = mod_exp(m, e, n);  // Encrypt the ASCII value using RSA
        printf("%lld ", encrypted[i]);  // Print encrypted values
    }
    printf("\n");

    // Step 8: Decrypt the message
    printf("\nDecrypted Message:\n");
    for (int i = 0; i < len; i++) {
        int decrypted = (int)mod_exp(encrypted[i], d, n);  // Decrypt the ASCII value using RSA
        printf("%c", (char)decrypted);  // Convert the decrypted ASCII value back to a character
    }
    printf("\n");

    return 0;
}
```
## Output:
![Screenshot 2024-10-16 144144](https://github.com/user-attachments/assets/3a18f9e7-b9b5-4485-99f9-332a0e81fba5)


## Result:
The program for RSA encryption and decryption is executed successfully.
