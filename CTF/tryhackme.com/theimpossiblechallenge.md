# The Impossible Challenge

- [Tools](#tools)
- [Task](#task-1)

Room Link: https://tryhackme.com/room/theimpossiblechallenge

## Tools 

1. Cryptography Decoder https://gchq.github.io
2. Zero Width Decoder https://330k.github.io/misc_tools/unicode_steganography.html

## Task 1

1. Got a zip file need to get password to unzip it.
2. After decoding hash on the main page, ROT13, ROT47, hex and base64 decoding lead to decoded text "It's inside the text, in front of your eyes!".
3. So, as in decoded text if we look inside text (source code) of the page, we can find unusual encoding around "Hmm".
4. There is zero width text, decoded with https://330k.github.io/misc_tools/unicode_steganography.html and got the password, "Password is *******".
5. Used the password and unzipped the zip file to get the flag.

Author: Zishan Ahamed Thandar
