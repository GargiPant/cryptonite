# PicoCTF Writeups

This repository contains writeups for various PicoCTF challenges. Each entry documents the problem-solving process, tools used, and final flags.

---



## 1. Keygenme-py

**Category:** Reverse Engineering  


**Challenge Description:**
The script contains a license key template:  
- Static part: `picoCTF{1n_7h3_|<3y_of_`
- Dynamic part: 8 characters (calculated)
- Ending: `}`
  
![image](https://github.com/user-attachments/assets/9c049d15-9050-42ea-97fc-7d55adc89e0b)

We are asked to reverse-engineer the logic to derive the correct license key.

**Approach:**
From the `menu_trial()` function, we find an option called "Enter License Key."  
The key validation function checks if the key length is correct, and if each character matches the expected string.
![image](https://github.com/user-attachments/assets/7b57e648-8357-405b-96f5-7c5d74223cee)
![image](https://github.com/user-attachments/assets/e17ae058-b9cf-441f-bc15-fea7afd8dcb6)

The `check key` functions first check the length of the key inputted by the user is same as the key then compares each character of the key to the key imputed by the user 
![image](https://github.com/user-attachments/assets/d0363766-8071-4231-851e-b910b41e1e63)

We Create a python script to calculate the dynamic part of the key based on the given username.
![image](https://github.com/user-attachments/assets/ee2308e2-92f9-45c9-a971-8a11aab1b3c6)

Upon executing the code, we get the key to be `picoCTF{1n_7h3_|<3y_of_54ef6292}`. We can check if this is correct by running the program and inputting the code

![image](https://github.com/user-attachments/assets/0a5f46fd-0bc4-4667-98d3-969d8764735c)

### Key :picoCTF{1n_7h3_|<3y_of_54ef6292}
---
## 2. GDB Baby Step 1

**Category:** Reverse Engineering 

**Challenge Description:**
Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.
Disassemble this.

**Approach:**
This challenge is an executable file and our task is to do Reverse Engineering on it using GDB and find out the value contained in the EAX register at the end of the main function.

We check the content of the directory by ls command We check the file type of debugger0_a. We find its a ELF file, 64 bit Executable and not stripped 

Start GDB by running the following command.
gdb debugger0_a
Then, since we need to locate the main function, lets list all the functions.
 info functions
![image](https://github.com/user-attachments/assets/abb8cab6-625b-4c68-b038-b6b0131c794b)

The syntax for GDB is set to AT&T by default. To use Intel syntax, type the following command. `set disassembly-flavor intel`
To disassemble the assembly code of the main function in this file, we use the following command. `disassemble main`
We need to search for the contents of the "eax" register. The contents are in hexadecimal (`0x86342`).
![image](https://github.com/user-attachments/assets/5ed0fef1-bc8c-4b16-ab8f-40564cc93820)

Using python, we can easily convert the hexadecimal number to decimals. print(int(0x86342)) We get the number 549698 as decimal. Hence, the pico flag is picoCTF{549698}.

**KEY :picoCTF{549698}**
---

## 3 ARMssembly 0

**Category:** Reverse Engineering 

**Challenge Description:**
What integer does this program print with arguments 182476535 and 3742084308? File: chall.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})


**Approach:**
To solve this challenge we could potentially, read and understand the ARM assembly or we could compile this ARM assembly to binary and execute it , to get to know what this binary is assembly code is doing .
By analysing the program we found that we just have to convert the bigger number into the hexadecimal format se we do that using python 
![image](https://github.com/user-attachments/assets/b80206a8-4f5f-4305-a3c9-56f03bf97683)

**KEY: picoCTF{6d1d2dd1}**
---

## 4 Stonks

**Category:** Binary exploitation

**Challenge Description:**
I decided to try something noone else has before. I made a bot to automatically trade stonks for me using AI and machine learning. I wouldn't believe you if you told me it's unsecure! vuln.c nc mercury.picoctf.net 59616


**Approach:**
Downloaded the vuln.c file and connected to the given server. Look at the user_buf field. Upon running the program from the terminal we found some hexadecimal values
![image](https://github.com/user-attachments/assets/c2a21b2b-d1dc-4837-ab09-a3c5c1cd010e)

We convert the hexadecimal into ASCII string text 
![image](https://github.com/user-attachments/assets/6561d119-46cd-4fe2-8b18-921cd4063ded)

Used python to get the string in the right order 
![image](https://github.com/user-attachments/assets/35756069-9352-4135-bbac-1cb5fcae2676)

**KEY: picoCTF{I_l05t_4ll_my_m0n3y_bdc425ea}**

 ---
## 5. Buffer Overflow 0

**Category:** Binary exploitation

**Challenge Description:**
Smash the stack Let's start off simple, can you overflow the correct buffer? The program is available here. You can view source here. And connect with it using: nc saturn.picoctf.net 51110

**Approach:**

On opening the source code, we see that on line 40, the gets() function is called, and reads buf1 onto the stack. This function writes the user’s input to the stack without regard to its allocated length. The user can simply overflow this length, and the program will pass their input into the vuln() function to trigger a segmentation fault.
![image](https://github.com/user-attachments/assets/39770beb-8f7e-49dc-a0f4-cbb06325f009)

**KEY:picoCTF{ov3rfl0ws_ar3nt_that_bad_9f2364bc}**
---
## 6. Babygame 01

**Category:** Binary exploitation

**Challenge Description:**
Get the flag and reach the exit.

Welcome to BabyGame! Navigate around the map and see what you can find! The game is available to download here. There is no source available, so you'll have to figure your way around the map.


**Approach:**
Begin by running the binary to understand its behavior.Solving the game did not retrieve the flag.
![image](https://github.com/user-attachments/assets/ee198dac-2b70-4f66-a395-ff62b9c9b63b)

Open the code in an online decompiler to look at the source code.
![image](https://github.com/user-attachments/assets/86822d5b-cb7c-4f0a-89fc-1b6c43208499)

In the main function, we see that the flag will be printed once the variable 'local_aa4' is not a null value. To trigger this condition we must firstly win the game and the local variable  'local_aa4' must be non-zero to drop the flag.
![image](https://github.com/user-attachments/assets/39023235-9c92-4bca-ac44-8f4981b1bb9f)

'init_map()' confirms the purpose and ordering of the player position coordinates. It also exposes the exit position which is the game win condition, that is the player must reach {29, 89} = {0x1d, 0x59}.
![image](https://github.com/user-attachments/assets/ba78550d-61ef-478d-9f65-8080fac71948)

By manipulating the player's position values, we can control the array indexing and write data to a specific location in memory. 'local_aa4' is located on the stack, and it immediately precedes the 'map_buf[]' array. Underflowing the 'map_buf' array involves manipulating the array index in a way that we access memory before the beginning of the array. This can lead to unintended consequences, such as overwriting variables on the stack. By setting the player's position to {0, -4}, we are effectively moving the array index four positions backward. This positioning is crucial to writing data to 'local_aa4' on the stack. The player_title character is used to write data to the least significant byte (LSB) of 'local_aa4', effectively changing its value.
Going to position {0, -4} has effectively changed our flag variable value from 0 to 64.
Typing 'p' will effectively solve the map for us and display the flag.

![image](https://github.com/user-attachments/assets/ab9ef8d2-fd5f-4c23-9650-cbc33fd48a9f)
---
## 7. Caas

**Category:** Web Exploitation

**Challenge Description:**
This is a really weird text file TXT? Can you find the flag?

**Approach:**
Visit the provided URL for the CAAS challenge and explored it.Use the curl command in the terminal to visit the provided URL.
![image](https://github.com/user-attachments/assets/acfedde4-d3d0-47ee-9060-41deded5dea3)

we can pass the following command using the terminal.
 curl`https://caas.mars.picoctf.net/cowsay/{message};ls`;
This prints all the files in the directory `/usr/games/cowsay`.

![image](https://github.com/user-attachments/assets/8b456caf-da5f-4088-8cd6-addcdefe0a41)

The vulnerability in this code lies in the fact that it directly uses user input (req.params.message) to construct a command that is executed with 'exec'.By using the following command, we can effectively exploit this vulnerability to print the required flag.
 curl `https://caas.mars.picoctf.net/cowsay/{message};cat%20falg.txt`; 

![image](https://github.com/user-attachments/assets/567102d2-6efd-48ca-a399-4f7527939b87)
---
## 8. Forbidden Paths

**Category:** Web Exploitation

**Challenge Description:**
Can you get the flag?Here’s the website.We know that the website files live in /usr/share/nginx/html/ and the flag is at /flag.txt but the website is filtering absolute file paths. Can you get past the filter to read the flag?

**Approach:**
Visit the provided URL for the Forbidden Paths challenge and explore the functionality of the web application.
![image](https://github.com/user-attachments/assets/8d8fcd65-eae1-4255-8e05-84bdb44cce01)

We know that the files are present in the `/usr/share/nginx/html/` directory, and the flag.txt file is present in the root directory. So, to reach the root directory, and display the flag.txt file, we can use the following input: ../../../../flag.txt
![image](https://github.com/user-attachments/assets/254f1ec0-3851-4692-aa90-85ce7da1fdc1)
---
## 9. Local Authority

**Category:** Web Exploitation

**Challenge Description:**
Can you get the flag?
Additional details will be available after launching your challenge instance.

**Approach:**
Visit the provided URL for the Local Authority challenge and explore the functionality of the web application.
![image](https://github.com/user-attachments/assets/8145ec4d-4d2e-4074-b0f7-1e28e60fcb28)

After reaching the `http://saturn.picoctf.net:59126/login.php` URL, view the page's source code. Here, we can see a locally included file, "secure.js", which tells us our required username and password.
![image](https://github.com/user-attachments/assets/ea749aa9-a202-4b90-8803-0409ce401ccc)

We retrieve the flag after using username = "admin" and password ="strongPassword098765".
![image](https://github.com/user-attachments/assets/aa878e38-4413-43ae-b96a-58e4a95ea832)

## 10. Trivial File Transfer Protocol

**Category:** Forensics

**Challenge Description:**
Figure out how they moved the flag.

**Approach:**
Begin by downloading the file and opening using wireshark. We can see some TFTP traffic using this. We will extract these TFTP files to a local folder
![image](https://github.com/user-attachments/assets/1169e044-a74e-4337-b378-74cb53d24e9a)

6 files have been extracted to our folder. Out of these, 2 are .txt files, 3 are .bmp files, and 1 is a .deb file
![image](https://github.com/user-attachments/assets/29f00ef2-a086-40ff-b3a6-e4d4204ab3a4)

Looking through instructions.txt, we can put the text into an online cipher identifier. This shows that the text is encrypted in a ROT-13 cipher. So is the 'plan' file.
![image](https://github.com/user-attachments/assets/ac4f58b6-4538-41b6-a770-812094c57007)

Looking through the .deb file, we can see that the user has encrypted the flag using steghide. The plan file tells us that the flag is hidden in one of the images using the password 'DUEDILIGENCE'. Using the following command on all three images, we can find the flag file.
`steghide extract -sf picture1.bmp`
Cat-ing the contents of the new flag.txt file, we can acquire the flag. 
`cat flag.txt`

**KEY:picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}**
---

## 11. New Ceasar

**Challenge Description:**
We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{}) kjlijdliljhdjdhfkfkhhjkkhhkihlhnhghekfhmhjhkhfhekfkkkjkghghjhlhghmhhhfkikfkfhm new_caesar.py

**Approach:**
Upon analyzing the code, we see that the cipher takes place in two parts. One b16_encoder, and one shift.
Create a program to reverse both these functions.
![image](https://github.com/user-attachments/assets/65e66bff-5b23-424c-ad58-bdbae56d2e2f)

Create a program to reverse both these functions.
![image](https://github.com/user-attachments/assets/656e01d6-cd21-4f61-be79-29a802f44ce3)

**Key: picoCTF{et_tu?_1ac5f3d7920a85610afeb2572831daa8}**
---

## 12. Tunn3l v1s10n

**Challenge Description:**
We found this file. Recover the flag.

**Approach:**
Begin by downloading the file and checking its file type using the file command.
file tunn3l_v1s10n
![image](https://github.com/user-attachments/assets/161b892c-c3b6-4f4a-8366-ce2b22ec7051)

However, since the file command doesn't provide sufficient information about the file, we will use the mimetype command.
mimetype tunn3l_v1s10n 
![image](https://github.com/user-attachments/assets/14901144-e228-4022-b262-6a2f01835018)

Use the mv command to change the extension of the file to .bmp Since tunn3l_v1s10n is an image bitmap file, and it isn't displaying, we will view it in a hexeditor.
![image](https://github.com/user-attachments/assets/d2962028-b705-41ca-ad9e-3fecf0dd7092)

A few of the initial header values are incorrect, and need to be static values. Using the editor, we can edit these values to their correct forms.
![image](https://github.com/user-attachments/assets/b9c419b7-8d5f-45b4-bc88-7f347275d1cb)
Using the feh command, we can display the bitmap image. 
          `  feh tunn3l_v1s10n.bmp`
 ![image](https://github.com/user-attachments/assets/19da97d3-bb26-4985-aac8-8865f5c8866f)

This image still doesn't display the correct flag. Use the exiftool command to view the resolutions of the image.
          `exiftools tunn3l_v1s10n.bmp`
Changing the values in the HexEditor to match the hexadecimal value of 840 in little endian
![image](https://github.com/user-attachments/assets/44cc5d6f-6ae8-4aac-9178-b73d23069ba0)

Using the feh command to display the final image. The flag is displayed.
![image](https://github.com/user-attachments/assets/0d6e9d34-a883-41ed-9cfa-bd9861ff4b09)
