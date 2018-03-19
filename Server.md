Room messages are not encrypted and should be stored in some random database

Group chat messages are encrypted with a client generated key that is sent to other group members on addition or deletion of other members. messages stored in some random database

PM messages are encrypted with the other user's public key and are added directly to their account file on the account database


# Accounts
Accounts can be stored in any way desired on the server. However, a SQL database is desired.

Each user account will have the following information associated it:
* Username (only one username can exist on a server)
* Hashed password (benchmark for 1 second of iterations)
* Encrypted private key (from a key that is password-derived)
* Public key
* List of the rooms that the user belongs to (encrypted to just one string with a key that is password-derived)
* Group Chats & Private Messages (encrypted to just one string with a key that is password-derived)
  * Chat ID
  * Chat key
* Privelege (either normal or admin)
  * Every user is normal by default
  * A user cannot be promoted to admin by a command given over the network, only local changes
* TODO: PMs

# Rooms
Rooms are a place where any user with an account can connect–and thus are not encrypted. Room names can only contain spaces, A-Z (only uppercase), and 0-9. A server should ignore all other requests relating to rooms that do not match the above requirements.

In addition, a server implementer may choose whether or not there are only certain "valid" rooms. Typically, any name room that a user wants can be used to send messages. If a server implementer limits rooms to only "valid" rooms, then only rooms with certain names can be used for messaging.

# Group Chats (GCs) & Private Messages (PMs)
Due to the similar nature of GCs and PMs, they will be combined and act as the same on the server side. However, clients can distinguish between them if they wish.

Any user can create a GC/PM. Their client will generate the key and will request the server to pass it onto other users who the user adds to the chat.
