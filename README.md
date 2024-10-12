# EX.-NO-1-B-IMPLEMENTATION-OF-PLAYFAIR-CIPHER

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

void toLowerCase(char *text) {
    for (int i = 0; text[i]; i++) {
        text[i] = tolower(text[i]);
    }
}

void removeSpaces(char *text) {
    int count = 0;
    for (int i = 0; text[i]; i++) {
        if (text[i] != ' ') {
            text[count++] = text[i];
        }
    }
    text[count] = '\0';
}

void diagraph(char *text, char digraphs[][2]) {
    int i = 0, k = 0;
    while (text[i] != '\0') {
        digraphs[k][0] = text[i];
        if (text[i + 1] == '\0' || text[i] == text[i + 1]) {
            digraphs[k][1] = 'x';
            i++;
        } else {
            digraphs[k][1] = text[i + 1];
            i += 2;
        }
        k++;
    }
}

void fillerLetter(char *text) {
    if (strlen(text) % 2 != 0) {
        strcat(text, "x");
    }
}

void generateKeyTable(char *key, char keyTable[5][5], char *list1) {
    int dict[26] = {0};
    int k = 0;

    // Fill key letters
    for (int i = 0; key[i]; i++) {
        if (key[i] != 'j' && dict[key[i] - 'a'] == 0) {
            keyTable[k / 5][k % 5] = key[i];
            dict[key[i] - 'a'] = 1;
            k++;
        }
    }

    // Fill remaining letters
    for (int i = 0; list1[i]; i++) {
        if (list1[i] != 'j' && dict[list1[i] - 'a'] == 0) {
            keyTable[k / 5][k % 5] = list1[i];
            dict[list1[i] - 'a'] = 1;
            k++;
        }
    }
}

void search(char keyTable[5][5], char ch, int *row, int *col) {
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyTable[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encryptRowRule(char keyTable[5][5], int e1r, int e1c, int e2r, int e2c, char *c1, char *c2) {
    *c1 = keyTable[e1r][(e1c + 1) % 5];
    *c2 = keyTable[e2r][(e2c + 1) % 5];
}

void encryptColumnRule(char keyTable[5][5], int e1r, int e1c, int e2r, int e2c, char *c1, char *c2) {
    *c1 = keyTable[(e1r + 1) % 5][e1c];
    *c2 = keyTable[(e2r + 1) % 5][e2c];
}

void encryptRectangleRule(char keyTable[5][5], int e1r, int e1c, int e2r, int e2c, char *c1, char *c2) {
    *c1 = keyTable[e1r][e2c];
    *c2 = keyTable[e2r][e1c];
}

void encryptByPlayfairCipher(char keyTable[5][5], char digraphs[][2], int size, char *cipherText) {
    char c1, c2;
    int e1r, e1c, e2r, e2c;
    
    for (int i = 0; i < size; i++) {
        search(keyTable, digraphs[i][0], &e1r, &e1c);
        search(keyTable, digraphs[i][1], &e2r, &e2c);

        if (e1r == e2r) {
            encryptRowRule(keyTable, e1r, e1c, e2r, e2c, &c1, &c2);
        } else if (e1c == e2c) {
            encryptColumnRule(keyTable, e1r, e1c, e2r, e2c, &c1, &c2);
        } else {
            encryptRectangleRule(keyTable, e1r, e1c, e2r, e2c, &c1, &c2);
        }

        cipherText[2 * i] = c1;
        cipherText[2 * i + 1] = c2;
    }
    cipherText[2 * size] = '\0';
}

int main() {
    char text_Plain[] = "instruments";
    char key[] = "Monarchy";
    char list1[] = "abcdefghiklmnopqrstuvwxyz";
    char digraphs[10][2]; // Max size is half the length of text_Plain
    char keyTable[5][5];
    char cipherText[20]; // Max length is 2 * length of digraphs

    toLowerCase(text_Plain);
    removeSpaces(text_Plain);
    fillerLetter(text_Plain);
    diagraph(text_Plain, digraphs);

    generateKeyTable(key, keyTable, list1);
    
    int size = strlen(text_Plain) / 2;
    encryptByPlayfairCipher(keyTable, digraphs, size, cipherText);

    printf("Key text: %s\n", key);
    printf("Plain Text: %s\n", text_Plain);
    printf("CipherText: %s\n", cipherText);

    return 0;
}
```
## OUTPUT:
![image](https://github.com/user-attachments/assets/6474f4aa-5342-4881-8d3c-b880f1a0bf21)

## RESULT:
  Thus the Playfair cipher substitution technique had been implemented successfully.
