# Password Changes

This is a guide on how to change your _**CSL**_ password, and contains some good information for changing other peoples' _**CSL**_ passwords too (for those with access).

This guide has nothing to do with _**LOCAL**_ (ie, Windows) passwords, or fcps account passwords. Only Syslab passwords. LOCAL passwords can still be used to log into (some) TJ services but cannot be changed from the CSL network.

## Password Requirements

Before changing your password, please be aware that your password must meet the following security guidelines:

* Must be at least 8 characters long
* Must contain at least one character from 3 of the following 4 classes of characters
  * Upper Case letters
  * lower case letters
  * Numbers
  * Symbols
* Cannot contain more than two consecutive characters from your First or Last name or your Username (eg. username 2017awilliam could have 20 in the password but not 201).
* Cannot be the same as any of your previous 4 passwords.

We recommend that you use a unique password for your TJHSST Network Account. Please remember that you are responsible for _**ALL**_ activity on your account, even if you get hacked due to a weak password.

## Through `remote.tjhsst.edu`

### Using Windows

1. Download putty.exe from [http://www.chiark.greenend.org.uk/\~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/\~sgtatham/putty/download.html)
2. Run putty.exe (there is no installer)
3. In the box that opens, make sure the Connection type is set to SSH
4. In the Host Name field, type `remote.tjhsst.edu`
5. Click Open at the bottom of the window
6. The box will disappear and be replaced with a black window
7. At the `login as:` prompt, type your username and hit enter
8. A welcome message will be printed and then a password prompt will appear; type your current password and press enter to login. **Note that this is a Linux system and NOTHING will be printed as you type your password.**
9. If you are required to change your password, you will be immediately prompted to do so with the message `New Password:`. If so, proceed to step 13, if not proceed to step 10
10. An additional welcome message will be printed and you will then be at a prompt of the form \`username@ras2 \~ $\`\`
11. Type `kpasswd` and press enter
12. Type in your current password and then press enter
    * If you see a message: `kpasswd: Password incorrect` then you did not enter your old password correctly, please return to step 10 and try again.
13. Type in a new password and then press enter
14. Type in the same new password again and then press enter
15. If you see a message `Success : Password changed` then you have successfully changed your password, you may close PuTTY by clicking on the red X in the upper right corner.
16. If you see a message `Verify failure` or `Password mismatch` then you did not type the same new password at steps 13 and 14, please return to step 11 (step 13 if you are being forced to change your password) and try again.
17. If you see a message `Password change rejected` then your new password did not meet the Password Requirements, please return to step 11 (step 13 if you are being forced to change your password) and try again.

### Using MacOS

1. Open the Terminal application from the Application Launcher
2. In the terminal, type `ssh your-tj-username@remote.tjhsst.edu`
3. A welcome message will be printed and then a password prompt will appear; type your old password and then press enter. **Note that this is a Linux system and NOTHING will be printed as you type your password**
4. If you are required to change your password, you will be immediately prompted to do so with the message `New Password:`. If so, proceed to step 8, if not proceed to step 5
5. An additional welcome message will be printed and you will then be at a prompt of the form `username@ras2 ~ $`
6. Type `kpasswd` and press enter
7. Type in your current password and then press enter
   * If you see a message: `kpasswd: Password incorrect` then you did not enter your old password correctly, please return to step 6 and try again.
8. Type in a new password and then press enter
9. Type in the same new password again and then press enter
10. If you see a message `Success : Password changed` then you have successfully changed your password, you may close the Terminal Application.
11. If you see a message `Verify failure or Password mismatch` then you did not type the same new password at steps 8 and 9, please return to step 6 (step 8 if you are being forced to change your password) and try again.
12. If you see a message `Password change rejected` then your new password did not meet the Password Requirements, please return to step 6 (step 8 if you are being forced to change your password) and try again.

### Using Linux

1. If you are using Linux, you probably know how to change your password. If not, the instructions are exactly the same as MacOS

## Using a Workstation at TJHSST

### Windows Workstation (most computer labs)

Note: this will not change your _**CSL**_ password, only your _**LOCAL**_ password. Your LOCAL password can still be used to log into most CSL services, but only if you have not set a CSL password. If you need to reset your CSL password, see the Linux Workstation section or one of the remote.tjhsst.edu sections.

1. Press ctrl + alt + delete; then read the displayed warning and click OK.
2. Log into the workstation using your existing username and password
3. If you are required to change your password, you will be immediately prompted to do so. If so, proceed to step 5, if not proceed to step 4.
4. After the system has finished loading the desktop, press ctrl + alt + delete again and click on Change Password.
5. Enter your current password in the top field, then enter your desired new password into the next two fields. Finally click the button at the bottom to change your password.
6. If you receive an error message, please follow the instructions it provides; most likely you either did not type one of the passwords correctly or your password does not meet our security requirements.

### Linux Workstation (Computer Systems Lab)

1. Enter your username at the login prompt and press enter
2. Enter your password and press enter
3. If you are required to change your password, you will be immediately prompted to enter a new password, if so, proceed to step 4, if not proceed to step 6.
4. Enter your desired new password and press enter.
5. Enter your desired new password again and press enter.
6. If the system continues to log you on, your password has been changed successfully. If not please read and follow the instructions in any error message that is displayed.
7. If you were not prompted to change your password, read the login warning and then click OK.
8. After the system has finished loading, please open a terminal emulator. If you require assistance with this, please ask one of the Teachers or a Student Systems Administrator for assistance.
9. In the open terminal, type `kpasswd` and press enter.
10. Type in your current password and then press enter
    * If you see a message: `kpasswd: Password incorrect` then you did not enter your old password correctly, please return to step 7 and try again.
11. Type in a new password and then press enter
12. Type in the same new password again and then press enter
13. If you see a message `Success : Password changed` then you have successfully changed your password, you may close the Terminal Application.
14. If you see a message `Verify failure` then you did not type the same new password at steps 9 and 10, please return to step 7 and try again.

## Through Ion

You can't change your password through Ion. Please use one of the other methods listed here.
