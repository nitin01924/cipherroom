# CipherRoom Data Model

CipherRoom uses a relational data model to represent users, private
rooms, memberships, invitations, and encrypted messages.

The main entities are:

- User
- Room
- Membership
- Invitation
- Message

---

## 1. User

The `User` entity represents an account registered on CipherRoom.

### Fields

- `id` - Unique identifier for the user.
- `name` - User's display name.
- `email` - User's email address.
- `passwordHash` - Hashed password. Plaintext passwords must never be stored.
- `emailVerified` - Indicates whether the user's email address has been verified.
- `accountStatus` - Current status of the account, such as active or disabled.
- `role` - Application role, such as user or admin.
- `createdAt` - Account creation timestamp.
- `updatedAt` - Last update timestamp.

### Notes

Authentication and account-management information belongs to the User
entity.

Encryption keys and other cryptographic information will be designed
separately as part of the E2EE architecture.

---

## 2. Room

The `Room` entity represents a private conversation.

### Fields

- `id` - Unique identifier for the room.
- `createdBy` - References the user who created the room.
- `createdAt` - Room creation timestamp.
- `updatedAt` - Last update timestamp.

### Relationships

A room can have multiple members.

`createdBy` references:

```text
Room.createdBy → User.id
```

A room may also be referenced by many membership records and messages.

---

## 3. Membership

The `Membership` entity represents a user's participation in a room.

### Fields

- `id` - Unique identifier for the membership record.
- `userId` - References the user who belongs to the room.
- `roomId` - References the room the user is a member of.
- `role` - Member role within the room, such as admin or member.
- `status` - Current membership state, such as active, invited, left, or removed.
- `joinedAt` - Time the user joined the room.
- `createdAt` - Membership creation timestamp.
- `updatedAt` - Last membership update timestamp.

### Relationships

A user can have many room memberships, and each room can contain many members.

```text
Membership.userId → User.id
Membership.roomId → Room.id
```

`(userId, roomId)` should be unique to prevent duplicate room membership entries.

---

## 4. Invitation

The `Invitation` entity represents a pending or historical invite to join a room.

### Fields

- `id` - Unique identifier for the invitation.
- `roomId` - References the room being shared.
- `inviterId` - User who created the invitation.
- `inviteeEmail` - Email address of the invited user, when the invite is email-based.
- `inviteeUserId` - Optional reference to the invited user when they already have an account.
- `status` - Invitation state, such as pending, accepted, expired, or revoked.
- `tokenHash` - Hashed secure token used to verify the invitation link.
- `expiresAt` - Time when the invitation expires.
- `createdAt` - Invitation creation timestamp.
- `updatedAt` - Last invitation update timestamp.
- `respondedAt` - Time the invite was accepted, declined, or otherwise resolved.

### Relationships

An invitation is always tied to a room and usually created by a room member.

```text
Invitation.roomId → Room.id
Invitation.inviterId → User.id
Invitation.inviteeUserId → User.id
```

This entity supports room sharing workflows and tracks the lifecycle of invite-based access.

---

## 5. Message

The `Message` entity stores encrypted conversation data for a room.

### Fields

- `id` - Unique identifier for the message.
- `roomId` - References the room the message belongs to.
- `senderId` - User who sent the message.
- `ciphertext` - Encrypted message payload stored by the server.
- `nonce` - Cryptographic nonce or initialization value used for encryption.
- `messageType` - Type of content, such as text, image, or file.
- `replyToMessageId` - Optional reference to a previous message being replied to.
- `createdAt` - Message creation timestamp.
- `updatedAt` - Last update timestamp.
- `deletedAt` - Optional timestamp if the message was deleted.

### Relationships

Each message belongs to exactly one room and is sent by one user.

```text
Message.roomId → Room.id
Message.senderId → User.id
Message.replyToMessageId → Message.id
```

### Notes

CipherRoom stores only encrypted message content and metadata required for delivery and ordering. Plaintext message bodies are never persisted on the server.

The message entity is designed to support end-to-end encrypted communication while preserving the operational metadata needed for chat features such as threading, timestamps, and delivery status.