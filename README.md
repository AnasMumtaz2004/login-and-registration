Author : Anas Mumtaz
#include <bits/stdc++.h>
using namespace std;

void NewRegister();
bool validEmail(string);
bool validfirstname(string);
bool validlastname(string);
bool validpassword(string);
void verificationcode();
void Login();
void createUsername(string &username);
bool isSameUsername(string, string);
bool isSamePassword(string, string);
bool verifyCode(int);

string registeredUsername, registeredPassword;
int savedVerificationCode;

int main() {
    // User opens website
    cout << "Hey.." << endl;
    cout << "Welcome to my website" << endl << endl;
    NewRegister();
    return 0;
}

void NewRegister() {
    string NewRegister, firstname, lastname, email, password;
    
    cout << "New register? \n(Note: if you are a new user type Yes, otherwise type No) \n";
    cin >> NewRegister; 
    cin.ignore();
    
    if (NewRegister == "Yes" || NewRegister == "yes") {
        // First name
        cout << "First name: ";
        cin >> firstname;
        while (!validfirstname(firstname)) {
            cout << "The first name is not valid, please try again: ";
            cin >> firstname;
        }
        
        // Last name
        cout << "Last name: ";
        cin >> lastname;
        while (!validlastname(lastname)) {
            cout << "The last name is not valid, please try again: ";
            cin >> lastname;
        }
        
        // Email
        cout << "Email: ";
        cin >> email;
        while (!validEmail(email)) {
            cout << "The email address is not valid, please try again: ";
            cin >> email;
        }

        // Username creation
        cout << "Create your username: ";
        createUsername(registeredUsername);

        // Password
        cout << "Password (at least 1 digit, 1 special character, 1 uppercase, 1 lowercase, 8-15 characters): ";
        cin >> password;
        while (!validpassword(password)) {
            cout << "The password is not valid, please try again: ";
            cin >> password;
        }
        registeredPassword = password;

        cout << "Registration completed!" << endl;
        verificationcode();
    }
    else if (NewRegister == "No" || NewRegister == "no") {
        Login();
    }
}

bool validfirstname(string firstname) {
    return isupper(firstname[0]);
}

bool validlastname(string lastname) {
    return isupper(lastname[0]);
}

bool validEmail(string email) {
    int AT = -1, dot = -1;
    int counterforAT = 0, counterforDot = 0;
    if (isalpha(email[0])) {
        for (int i = 0; i < email.length(); i++) {
            if (email[i] == '@') {
                AT = i;
                ++counterforAT;
            }
            else if (email[i] == '.') {
                dot = i;
                ++counterforDot;
            }
        }
        if (AT == -1 || dot == -1 || AT > dot || counterforDot > 1 || counterforAT > 1 || dot >= (email.length() - 1)) {
            return false;
        }
        return true;
    }
    return false;
}

bool validpassword(string password) {
    int digit = 0, uppercase = 0, lowercase = 0, specialchar = 0;
    if (password.length() >= 8 && password.length() <= 15) {
        for (char ch : password) {
            if (isdigit(ch)) digit++;
            else if (islower(ch)) lowercase++;
            else if (isupper(ch)) uppercase++;
            else if (ch == '@' || ch == '#' || ch == '_') specialchar++;
        }
        return digit > 0 && uppercase > 0 && lowercase > 0 && specialchar > 0;
    }
    return false;
}

void createUsername(string &username) {
    cin >> username;
}

bool isSameUsername(string inputUsername, string registeredUsername) {
    return inputUsername == registeredUsername;
}

bool isSamePassword(string inputPassword, string registeredPassword) {
    return inputPassword == registeredPassword;
}

void verificationcode() {
    cout << "We have sent a verification code to your email to confirm your account." << endl
         << "Please check your email." << endl;

    savedVerificationCode = rand() % 9000 + 1000;  // generates a 4-digit code
    cout << "Email message: Your verification code is " << savedVerificationCode << endl;

    int code;
    cout << "Enter the verification code here: ";
    while (!(cin >> code) || !verifyCode(code)) {
        cout << "Incorrect verification code, please try again: ";
    }
    cout << "Your account has been verified." << endl;
    Login();
}

bool verifyCode(int inputCode) {
    return inputCode == savedVerificationCode;
}

void Login() {
    string inputUsername, inputPassword;
    int attempts = 0;

    cout << "     Log in       " << endl;
    cout << "Enter your username: ";
    cin.ignore();
    getline(cin, inputUsername);

    while (!isSameUsername(inputUsername, registeredUsername)) {
        cout << "Username not recognized. Please try again: ";
        getline(cin, inputUsername);
    }

    while (attempts < 3) {
        cout << "Enter your password: ";
        cin >> inputPassword;

        if (isSamePassword(inputPassword, registeredPassword)) {
            cout << "Login success!" << endl;
            return;
        } else {
            attempts++;
            cout << "Incorrect password. You have " << (3 - attempts) << " attempts left." << endl;
        }
    }

    cout << "Too many incorrect attempts. Account has been erased." << endl;
    registeredUsername = "";
    registeredPassword = "";
}
