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
