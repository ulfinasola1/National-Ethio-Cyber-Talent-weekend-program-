# Steganography Challenge Write‑Up

Challenge File: challenge.jpg

Step 1 — Strings Analysis
First, I ran the strings command on the image to check for readable text:
strings challenge.jpg | grep FLAG
I found the following flags:
FLAG{ALWAYS_Check_Metadata}
FLAG{Strings_leak_information}

Step 2 — Binwalk Analysis
Next, I used binwalk to see if there were any hidden files or archives in the image:
binwalk challenge.jpg
The output showed a ZIP archive appended to the image:
DECIMAL       HEXADECIMAL     DESCRIPTION
0             0x0             JPEG image data, JFIF standard 1.01
75667         0x12793         Zip archive data, name: flag4.txt

Step 3 — Extract Embedded Files
I extracted the embedded archive using:
binwalk -e challenge.jpg
After extraction, a new folder _challenge.jpg-0.extracted was created. I navigated into it:
cd _challenge.jpg-0.extracted

Step 4 — Read Extracted File
Inside the folder, I opened the extracted file:
cat flag4.txt
The content looked like Base64:
Umt4QlIzdGhjSEJsYm1SbFpGOTZhWEI5Q2c9PQo=

Step 5 — Decode Base64 (First Layer)
I decoded the Base64 string once:
echo "Umt4QlIzdGhjSEJsYm1SbFpGOTZhWEI5Q2c9PQo=" | base64 -d
The result was another Base64 string:
RkxBR3thcHBlbmRlZF96aXB9Cg==

Step 6 — Decode Base64 (Second Layer)
I decoded the string again:
echo "RkxBR3thcHBlbmRlZF96aXB9Cg==" | base64 -d
Finally, I obtained the third flag:
FLAG{appended_zip}

Step 7 — Steghide Bruteforce
I tested the image for hidden content using stegseek with the rockyou wordlist:
stegseek challenge.jpg /usr/share/wordlists/rockyou.txt
The passphrase was discovered: password123
The hidden file was revealed to be flag5.txt.

Step 8 — Extract Hidden File
I extracted the hidden file using steghide:
steghide extract -sf challenge.jpg
I entered the passphrase password123 and the file flag5.txt was successfully extracted.

Step 9 — Read Final Flag
I opened the extracted file:
cat flag5.txt
The final flag was:
FLAG{steghide_protected}

Final Flags Collected
FLAG{ALWAYS_Check_Metadata}
FLAG{Strings_leak_information}
FLAG{appended_zip}
FLAG{steghide_protected}

Tools I Used
strings
grep
binwalk
base64
stegseek
steghide

Author ULFINA TAKELE

