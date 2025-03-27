## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 ```
NAME : VINODINI R  
REG NO : 212223040244
```

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




## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define MX 5

// Function to prepare plaintext (removes spaces, replaces 'J' with 'I', etc.)
void prepareText(char str[]) {
    int len = strlen(str);
    int i, j = 0;
    // Convert to uppercase and replace 'J' with 'I'
    for (i = 0; i < len; i++) {
        if (str[i] == 'j' || str[i] == 'J') {
            str[j++] = 'I';  // Replace 'J' with 'I'
        } else if (isalpha(str[i])) {
            str[j++] = toupper(str[i]);  // Convert to uppercase
        }
    }
    str[j] = '\0';  // Null-terminate the string
    // If the length is odd, append an 'X' to make it even
    if (j % 2 != 0) {
        str[j++] = 'X';
    }
    str[j] = '\0';
}

// Function to generate the Playfair cipher key table
void generateKeyTable(char key[], char keyTable[MX][MX]) {
    int i, j, k = 0;
    int used[26] = {0};  // To mark already used letters in the key
    // Add the key to the table
    for (i = 0; i < strlen(key); i++) {
        if (!used[key[i] - 'A']) {  // If the letter is not already used
            keyTable[k / MX][k % MX] = key[i];
            used[key[i] - 'A'] = 1;
            k++;
        }
    }
    // Fill the remaining spaces in the table with unused letters
    for (i = 0; i < 26; i++) {
        if (!used[i] && i != ('J' - 'A')) {  // Skip 'J'
            keyTable[k / MX][k % MX] = 'A' + i;
            k++;
        }
    }
}

// Function to find the position of a character in the key table
void findPosition(char keyTable[MX][MX], char ch, int *row, int *col) {
    for (int i = 0; i < MX; i++) {
        for (int j = 0; j < MX; j++) {
            if (keyTable[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Function to perform encryption with the Playfair cipher
void playfair(char str[], char keyTable[MX][MX]) {
    int i;
    int row1, col1, row2, col2;

    FILE *out = fopen("cipher.txt", "w");  // Open the output file
    if (out == NULL) {
        printf("Error opening file.\n");
        return;
    }

    for (i = 0; i < strlen(str); i += 2) {
        findPosition(keyTable, str[i], &row1, &col1);
        findPosition(keyTable, str[i + 1], &row2, &col2);

        // If both characters are in the same row
        if (row1 == row2) {
            printf("%c%c", keyTable[row1][(col1 + 1) % MX], keyTable[row2][(col2 + 1) % MX]);
            fprintf(out, "%c%c", keyTable[row1][(col1 + 1) % MX], keyTable[row2][(col2 + 1) % MX]);
        }
        // If both characters are in the same column
        else if (col1 == col2) {
            printf("%c%c", keyTable[(row1 + 1) % MX][col1], keyTable[(row2 + 1) % MX][col2]);
            fprintf(out, "%c%c", keyTable[(row1 + 1) % MX][col1], keyTable[(row2 + 1) % MX][col2]);
        }
        // If neither, form a rectangle
        else {
            printf("%c%c", keyTable[row1][col2], keyTable[row2][col1]);
            fprintf(out, "%c%c", keyTable[row1][col2], keyTable[row2][col1]);
        }
    }
    fclose(out);  // Close the output file
}

int main() {
    char key[25], str[25], keyTable[MX][MX];

    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';  // Remove trailing newline character

    printf("Enter the plain text: ");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = '\0';  // Remove trailing newline character

    // Prepare the key and plaintext
    prepareText(key);
    prepareText(str);

    // Generate the Playfair cipher key table
    generateKeyTable(key, keyTable);

    // Display the generated key table
    printf("\nGenerated Key Table:\n");
    for (int i = 0; i < MX; i++) {
        for (int j = 0; j < MX; j++) {
            printf("%c ", keyTable[i][j]);
        }
        printf("\n");
    }

    printf("\nCipher Text: ");
    // Perform encryption
    playfair(str, keyTable);

    return 0;
}
```




# OUTPUT:


<img width="576" alt="c2" src="https://github.com/user-attachments/assets/a4a4a140-f92a-447c-8467-986036b5e094" />


## RESULT:

The implementation of Playfair Substitution technique using C program is successfully executed.
