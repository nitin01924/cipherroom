# CipherRoom Threat Model

This CipherRoom is a chating system, that provides the complete end to end security with the private encryption that even the developer can't access the messages of other users. 

## Purpose

 ### 1. the purpose of cipherroom is just to secure the chat.
 ### 2. we use end to end encryption and the key would just share to the users who are chatting each other.
...

## Privacy Goal
1. we are trying to protect the conversations of the user to the another user.
2. and trying to build a system where anyone can rely that the privacy of their messages are totally safe. 
...

## Server Can Access

1. user's details like : name, email, password (hashed), is verified, creation and updation timing.
2. the rooms details like : what's the room id, who are the users in that room, does the user able and authorized to access the room. 
- ...

## Server Must Not Access
1. the server should not excess the private and credential things of user, like password in plain text without hashing.
2. the encryption key for the decryption of messages.
3. the messages in plain text.
- ...

## Message Flow

first the user would be insures that it is the authanticated -> then he write a message -> then the cipher encryptes it by using that key -> then it goes to the server and genereates a request to the server to save -> then the server take it and save to the database -> then send it to that user and where that encrypted message will be decrypted -> because that user have the encryption key. 
...

## Attachment Flow

before reaches the cloudinary, the image should have to be encrypted, because our whole aim is to get the user's conversation completely private, so the encryption of the image is also a thing to be consider for security. and a image could have too much data, so it should be encrypted first then reaches to the cloudinary and then cloudinary accept it as this, and stores to itself with encryption.
...