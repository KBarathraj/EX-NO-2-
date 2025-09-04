## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
Program developed by BARATHRAJ K (212224230033)
```

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5
char keyTable[SIZE][SIZE];
void generateKeyTable(char key[]) {
    int used[26] = {0}; 
    int i, j, k = 0;
    for (i = 0; key[i]; i++) {
        char c = toupper(key[i]);
        if (c == 'J') c = 'I';
        if (c < 'A' || c > 'Z') continue; 
        if (!used[c - 'A']) {
            keyTable[k / SIZE][k % SIZE] = c;
            used[c - 'A'] = 1;
            k++;
        }
    }
    for (i = 0; i < 26; i++) {
        if (i + 'A' == 'J') continue;
        if (!used[i]) {
            keyTable[k / SIZE][k % SIZE] = i + 'A';
            k++;
        }
    }
}

void findPos(char c, int *row, int *col) {
    if (c == 'J') c = 'I';
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == c) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}
void prepareText(char *text, char *output) {
    int len = strlen(text), i, j = 0;
    for (i = 0; i < len; i++) {
        char c = toupper(text[i]);
        if (c < 'A' || c > 'Z') continue;
        if (c == 'J') c = 'I';
        if (j > 0 && output[j - 1] == c) {
            output[j++] = 'X';
        }
        output[j++] = c;
    }
    if (j % 2 != 0) output[j++] = 'X';
    output[j] = '\0';
}
void encryptText(char *text, char *cipher) {
    int len = strlen(text);
    for (int i = 0; i < len; i += 2) {
        int r1, c1, r2, c2;
        findPos(text[i], &r1, &c1);
        findPos(text[i+1], &r2, &c2);
        if (r1 == r2) { 
            cipher[i] = keyTable[r1][(c1 + 1) % SIZE];
            cipher[i+1] = keyTable[r2][(c2 + 1) % SIZE];
        } else if (c1 == c2) {
            cipher[i] = keyTable[(r1 + 1) % SIZE][c1];
            cipher[i+1] = keyTable[(r2 + 1) % SIZE][c2];
        } else { // rectangle
            cipher[i] = keyTable[r1][c2];
            cipher[i+1] = keyTable[r2][c1];
        }
    }
    cipher[len] = '\0';
}
void decryptText(char *cipher, char *plain) {
    int len = strlen(cipher);
    for (int i = 0; i < len; i += 2) {
        int r1, c1, r2, c2;
        findPos(cipher[i], &r1, &c1);
        findPos(cipher[i+1], &r2, &c2);
        if (r1 == r2) { 
            plain[i] = keyTable[r1][(c1 + SIZE - 1) % SIZE];
            plain[i+1] = keyTable[r2][(c2 + SIZE - 1) % SIZE];
        } else if (c1 == c2) { 
            plain[i] = keyTable[(r1 + SIZE - 1) % SIZE][c1];
            plain[i+1] = keyTable[(r2 + SIZE - 1) % SIZE][c2];
        } else { 
            plain[i] = keyTable[r1][c2];
            plain[i+1] = keyTable[r2][c1];
        }
    }
    plain[len] = '\0';
}
int main() {
    char key[100], text[100], prepared[200], cipher[200], decrypted[200];
    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';
    generateKeyTable(key);
    printf("Enter plaintext: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = '\0';
    prepareText(text, prepared);
    encryptText(prepared, cipher);
    printf("Encrypted: %s\n", cipher);
    decryptText(cipher, decrypted);
    printf("Decrypted: %s\n", decrypted);
    return 0;
}
```


Output:
<img width="2241" height="1326" alt="image" src="https://github.com/user-attachments/assets/b5c8cdc0-ede9-4d87-8248-f5679be054dd" />
