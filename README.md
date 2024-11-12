# EX.2-IMPLEMENTATION-OF-PLAYFAIR-CIPHER

## AIM:
  To write a C program to implement the Playfair Substitution technique.
  
## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

char keyMatrix[SIZE][SIZE];

// Function to remove duplicate characters from the keyword
void removeDuplicates(char *str) {
    int len = strlen(str);
    for (int i = 0; i < len; i++) {
        for (int j = i + 1; j < len;) {
            if (str[i] == str[j]) {
                for (int k = j; k < len; k++) {
                    str[k] = str[k + 1];
                }
                len--;
            } else {
                j++;
            }
        }
    }
}

// Function to generate the key matrix
void generateKeyMatrix(char *key) {
    char alphabet[] = "ABCDEFGHIKLMNOPQRSTUVWXYZ"; // I/J combined
    removeDuplicates(key);
    strcat(key, alphabet);

    int used[26] = {0}; // Track used characters
    int x = 0, y = 0;
    for (int i = 0; i < strlen(key); i++) {
        if (key[i] == 'J') key[i] = 'I';
        if (!used[key[i] - 'A']) {
            keyMatrix[x][y++] = key[i];
            used[key[i] - 'A'] = 1;
            if (y == SIZE) {
                y = 0;
                x++;
            }
        }
    }
}

// Function to find character position in the key matrix
void findPosition(char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';  // Treat 'J' as 'I'
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (keyMatrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Encryption function
void encrypt(char *plaintext, char *ciphertext) {
    for (int i = 0; i < strlen(plaintext); i += 2) {
        if (i + 1 == strlen(plaintext) || plaintext[i] == plaintext[i + 1]) {
            strcat(plaintext, "X");  // Add padding if necessary
        }
        int row1, col1, row2, col2;
        findPosition(plaintext[i], &row1, &col1);
        findPosition(plaintext[i + 1], &row2, &col2);

        if (row1 == row2) { // Same row
            ciphertext[i] = keyMatrix[row1][(col1 + 1) % SIZE];
            ciphertext[i + 1] = keyMatrix[row2][(col2 + 1) % SIZE];
        } else if (col1 == col2) { // Same column
            ciphertext[i] = keyMatrix[(row1 + 1) % SIZE][col1];
            ciphertext[i + 1] = keyMatrix[(row2 + 1) % SIZE][col2];
        } else { // Rectangle swap
            ciphertext[i] = keyMatrix[row1][col2];
            ciphertext[i + 1] = keyMatrix[row2][col1];
        }
    }
    ciphertext[strlen(plaintext)] = '\0';
}

// Decryption function
void decrypt(char *ciphertext, char *plaintext) {
    for (int i = 0; i < strlen(ciphertext); i += 2) {
        int row1, col1, row2, col2;
        findPosition(ciphertext[i], &row1, &col1);
        findPosition(ciphertext[i + 1], &row2, &col2);

        if (row1 == row2) { // Same row
            plaintext[i] = keyMatrix[row1][(col1 - 1 + SIZE) % SIZE];
            plaintext[i + 1] = keyMatrix[row2][(col2 - 1 + SIZE) % SIZE];
        } else if (col1 == col2) { // Same column
            plaintext[i] = keyMatrix[(row1 - 1 + SIZE) % SIZE][col1];
            plaintext[i + 1] = keyMatrix[(row2 - 1 + SIZE) % SIZE][col2];
        } else { // Rectangle swap
            plaintext[i] = keyMatrix[row1][col2];
            plaintext[i + 1] = keyMatrix[row2][col1];
        }
    }
    plaintext[strlen(ciphertext)] = '\0';
}

int main() {
    char key[] = "MONARCHY";
    char plaintext[] = "LOKESH";
    char ciphertext[128];
    char decryptedtext[128];

    // Generate the key matrix
    generateKeyMatrix(key);

    // Print the key matrix
    printf("Key Matrix:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", keyMatrix[i][j]);
        }
        printf("\n");
    }

    // Encrypt the plaintext
    encrypt(plaintext, ciphertext);
    printf("\nEncrypted text: %s\n", ciphertext);

    // Decrypt the ciphertext
    decrypt(ciphertext, decryptedtext);
    printf("Decrypted text: %s\n", decryptedtext);

    return 0;
}

```
## OUTPUT:

![image](https://github.com/user-attachments/assets/be29b6d5-6464-4fc3-a227-511b1d1f5ce7)


## RESULT:
  Thus the Playfair cipher substitution technique had been implemented successfully.
