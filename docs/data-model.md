## Data-Model 
- User
- Room
- Membership
- Invitation
- Message

## User
the user should have the info about the user for our database. and the main things would be like this:
1. name 
2. email
3. hashed passowrd
4. isVerified
5. jwt expiry
6. other metadata like when was the user created, etc.

## Room
the room is refers to the system where only two users can interact with each other at real time, in a secured and safe room. one of them will be send the request to another to join it, and then accept the request of joining or connecting if that user is ready.
1. room id
2. user id who created 
3. users id, who's are live in that room
4. is user verified.

## Membership
this model would be insures that the members of the room are valid or who's the member of which room and with whom.
1. user id
2. room id
3. member_with id

## Invitation 
1. isVerified (user who create the room)
2. before sending email to another user for joining, make sure that is that user is available on our platform, by using the user_exist (from email)
3. send email to another user 
4. does the another user is authenticated 
5. Invitation_id or something like token

## Message 
1. sending_time
2. message
