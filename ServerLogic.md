# Accounts
Accounts can be stored in any way desired on the server. However, a SQL database would be a good idea.

Each user account will have the following information associated it:
* Username
* Hashed password (100,000+ (TBD) iterations of PBKDF2)
* AES Encrypted (bit tbd) (from password) RSA private key
* RSA Public key
* Rooms
  * Room name
  * Password encrypted with user's password
* Group Chats & DMs
  * Chat ID
  * Chat key encrypted with RSA
