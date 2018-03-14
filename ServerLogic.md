# Accounts
Accounts can be stored in any way desired on the server. However, a SQL database would be a good idea.

Each user account will have the following information associated it:
* Username
* Hashed password (100,000+ (TBD) iterations of PBKDF2 -- benchmark .9 seconds for iteration ) (also look into bcrypt)
* AES Encrypted (tbd-bit) (from password) RSA private key
* RSA Public key
* List of the rooms that the user belongs too
* Group Chats & Private Messages
  * Chat ID
  * Chat key encrypted with RSA

# Rooms
Rooms are a place where any user with an account can connect–and thus are not encrypted. Room names can only contain spaces, A-Z (only uppercase), and 0-9. A server should ignore all other requests relating to rooms that do not match the above requirements.

In addition, a server implementer may choose whether or not there are only certain "valid" rooms. Typically, any name room that a user wants can be used to send messages. If a server implementer limits rooms to only "valid" rooms, then only rooms with certain names can be used for messaging.

# Group Chats (GCs) & Private Messages (PMs)
Due to the similar nature of GCs and PMs, they will be combined and act as the same on the server side. However, clients can distinguish between them if they wish.

