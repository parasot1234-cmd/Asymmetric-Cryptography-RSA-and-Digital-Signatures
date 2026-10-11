# Asymmetric Cryptography and Digital Signatures using OpenSSL

This project demonstrates the practical application of **Asymmetric Cryptography (RSA) and **Digital Signatures** using OpenSSL. It express communication scenario between two entities, **Alice** and **Bob**, .




 Linux (WSL)
 OpenSSL 





### 📌 Part 1: Key Generation
Before establishing secure communication, Bob will create a cryptographic key pair consisting of a private key and a public key.


# step 1.1: Generate a secure 2048-bit RSA Private Key for Bob
openssl genrsa -out private.pem 2048

# step 1.2: find the Public Key from Bob's Private Key
openssl rsa -in private.pem -pubout -out public.pem



 Part 2: Asymmetric Encryption & Decryption
Bob shares his `public.pem` file online. Alice uses this public key to lock her confidential file, ensuring that only Bob can unlock it using his secret private key.


#Step 2.1: Alice creates a secret plaintext file
echo "Hello Bob! This is my ultra secret message." > Secretfile.txt

#step 2.2: Alice encrypts the file using Bob's Public Key (Generates Ciphertext)
openssl pkeyutl -encrypt -pubin -inkey public.pem -in Secretfile.txt -out encrypted.txt

step 2.3: Bob decrypts the ciphertext using his own Private Key (Recovers Plaintext)
openssl pkeyutl -decrypt -inkey private.pem -in encrypted.txt -out Secretfile2.txt

note: Run `cat encrypted.txt` to view the unreadable ciphertext, and `cat Secretfile2.txt` to view the successfully recovered message.*



  part 3: Digital Signatures and Verification
To prove the authentiaction of communication, Alice digitally signs the document using her key so Bob can verify that the file has not been changed..


# Step 3.1: Alice digitally signs the document using her private key
openssl dgst -sha256 -sign private.pem -out sign.sha256 Secretfile.txt

#Step 3.2: Convert the binary signature to Base64 format for safe internet transmission
openssl base64 -in sign.sha256 -out signature.txt

#Step 3.3: Bob receives the signature and decodes  to binary format
openssl base64 -d -in signature.txt -out sign_verification.sha256

#Step 3.4: Bob verifies the signature using Alice's Public Key
openssl dgst -sha256 -verify public.pem -signature sign_verification.sha256 Secretfile.txt

  Output
If the file integrity is ok and the signature matches, OpenSSL will print:
text
Verified OK

If the file or signature was changed  , it will output `Verification Failure`.

