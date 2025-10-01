#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define KEY 5  // encryption key

// Function to encrypt/decrypt a string
void encryptDecrypt(char *str) {
    for (int i = 0; str[i] != '\0'; i++) {
        str[i] = str[i] ^ KEY;  // XOR encryption
    }
}

// Function to add a new password entry
void addPassword() {
    FILE *fp = fopen("passwords.txt", "a");
    char site[50], user[50], pass[50];

    printf("Enter site: ");
    scanf("%s", site);
    printf("Enter username: ");
    scanf("%s", user);
    printf("Enter password: ");
    scanf("%s", pass);

    encryptDecrypt(pass); // encrypt before saving
    fprintf(fp, "%s %s %s\n", site, user, pass);
    fclose(fp);

    printf("Saved!\n");
}

// Function to view saved passwords
void viewPasswords() {
    FILE *fp = fopen("passwords.txt", "r");
    if (!fp) { 
        printf("No data.\n"); 
        return; 
    }

    char site[50], user[50], pass[50];
    while (fscanf(fp, "%s %s %s", site, user, pass) != EOF) {
        encryptDecrypt(pass); // decrypt before showing
        printf("Site: %s | User: %s | Pass: %s\n", site, user, pass);
    }
    fclose(fp);
}

// Main program menu
int main() {
    int choice;
    while (1) {
        printf("\n1. Add Password\n2. View Passwords\n3. Exit\nChoice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: addPassword(); break;
            case 2: viewPasswords(); break;
            case 3: exit(0);
            default: printf("Invalid choice!\n");
        }
    }
    return 0;
}
