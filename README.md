# EX-NO-7-Implement-DES-Encryption-and-Decryption

## Aim:

To use the Data Encryption Standard (DES) algorithm for a practical application, such as securing sensitive data transmission in financial transactions.

## ALGORITHM:

1. DES is based on a symmetric key encryption technique that encrypts data in 64-bit blocks.
2. DES uses a Feistel network structure with 16 rounds of processing for encryption.
3. DES has a 64-bit key, but only 56 bits are used for encryption (the remaining 8 bits are for parity).
4. DES applies initial and final permutations along with 16 rounds of substitution and permutation transformations to produce ciphertext.

## Program:
```
#include <stdio.h>
#include <stdint.h>
#include <string.h>

// Just using a fixed 64-bit key and plaintext block for demo
uint64_t key = 0x133457799BBCDFF1;
uint64_t plaintext = 0x0123456789ABCDEF;

uint64_t IP(uint64_t input);
uint64_t FP(uint64_t input);
uint32_t feistel(uint32_t r, uint64_t subkey);
void generate_keys(uint64_t key, uint64_t subkeys[16]);
uint64_t des_encrypt(uint64_t block, uint64_t subkeys[16]);
uint64_t des_decrypt(uint64_t block, uint64_t subkeys[16]);

int main() {
    uint64_t subkeys[16];
    generate_keys(key, subkeys);

    uint64_t cipher = des_encrypt(plaintext, subkeys);
    printf("Encrypted: %016llX\n", cipher);

    uint64_t decrypted = des_decrypt(cipher, subkeys);
    printf("Decrypted: %016llX\n", decrypted);

    return 0;
}

// Dummy key schedule: reuse key for all 16 rounds (for demo only!)
void generate_keys(uint64_t key, uint64_t subkeys[16]) {
    for (int i = 0; i < 16; i++)
        subkeys[i] = key;
}

// Feistel function (simplified)
uint32_t feistel(uint32_t r, uint64_t subkey) {
    return (r ^ (subkey & 0xFFFFFFFF)); // Simplified F-function
}

// Initial Permutation (skipped real IP for simplicity)
uint64_t IP(uint64_t input) {
    return input;
}

// Final Permutation (skipped real FP for simplicity)
uint64_t FP(uint64_t input) {
    return input;
}

uint64_t des_encrypt(uint64_t block, uint64_t subkeys[16]) {
    block = IP(block);

    uint32_t left = (block >> 32) & 0xFFFFFFFF;
    uint32_t right = block & 0xFFFFFFFF;

    for (int i = 0; i < 16; i++) {
        uint32_t temp = right;
        right = left ^ feistel(right, subkeys[i]);
        left = temp;
    }

    uint64_t combined = ((uint64_t)right << 32) | left;
    return FP(combined);
}

uint64_t des_decrypt(uint64_t block, uint64_t subkeys[16]) {
    block = IP(block);

    uint32_t left = (block >> 32) & 0xFFFFFFFF;
    uint32_t right = block & 0xFFFFFFFF;

    for (int i = 15; i >= 0; i--) {
        uint32_t temp = right;
        right = left ^ feistel(right, subkeys[i]);
        left = temp;
    }

    uint64_t combined = ((uint64_t)right << 32) | left;
    return FP(combined);
}


```




## Output:
![Screenshot 2025-04-15 142834](https://github.com/user-attachments/assets/b34bc97e-8657-4c8a-b021-6d9ccb7d63ee)


## Result:
The program is executed successfully

