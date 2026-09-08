# CipherRoom Threat Model

## Purpose

CipherRoom is a privacy-focused real-time messaging system.
Its primary goal is to provide end-to-end encrypted private
communication between authorized users.

## Privacy Goal

The primary privacy goal is to prevent the CipherRoom server,
database, and storage providers from accessing plaintext message
contents or the cryptographic keys required to decrypt them.

## Server Can Access

- User identity information such as name and email
- Password hashes
- Email verification status
- Account status
- Authentication/session information
- Room identifiers
- Room membership
- Invitation information
- Authorization information
- Operational metadata such as message timestamps and delivery status
- Encrypted message ciphertext
- Encrypted attachments

## Server Must Not Access

- Plaintext passwords as persistent data
- Private encryption keys
- Plaintext message contents
- Plaintext image/file attachments
- Keys required to decrypt private conversations

## Message Flow

1. The user authenticates with CipherRoom.
2. The user writes a message in the browser.
3. The message is encrypted on the user's device.
4. Only the resulting ciphertext is sent to the server.
5. The server stores and/or relays the ciphertext.
6. The recipient receives the ciphertext.
7. The recipient's device decrypts the message locally.

## Attachment Flow

1. The user selects an attachment.
2. The attachment is encrypted locally in the browser.
3. The encrypted attachment is uploaded to storage.
4. The server stores metadata/reference information.
5. The recipient downloads the encrypted attachment.
6. The recipient's browser decrypts it locally.

## Threat Model Assumptions

CipherRoom assumes that the user's device and browser are not
compromised. End-to-end encryption protects message contents from
the server and storage infrastructure, but cannot protect plaintext
after it has been decrypted on a compromised device.

## Metadata

The server may have access to metadata required to operate the
service, including room membership, timestamps, delivery information,
and other operational information. Message content remains encrypted.