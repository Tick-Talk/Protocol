# Considerations During Implementation
Server creators have to keep many different security considerations in mind when implementing a server, for obvious reasons. When implementing a `DataType` in DataTransmission.md, a developer should always think:
* "Does this user have adequate priveleges to do the action they are requesting?"
* "Which logged-in users need to be sent a new `UserData` as a result of this `DataType` being executed?"
* "How can I make this as secure as possible?"

# Accounts
### Account Creation and Deletion
Account creation will happen on a webserver which has access to the account database. A simple PHP page will function to create, delete, and manage accounts. This is technically a part of the server that will be implemented. It is vital that the webserver uses (secure) SSL/HTTPS--this part is up to a server owner.

### Account Database
Accounts can be stored in any way desired on the server. However, a SQL database is recommended and desired.

Each user account will have the following information associated it:
* Username (only one username can exist on a server)
* Hashed password (benchmark for 1 second of iterations, or just do whatever libsodium says...look into this)
* Encrypted private key (private key is encrypted with a password-derived key--make sure this password derived key is NOT the same as the hashed password that is being stored, or anything remotely close; perhaps a different algorithm–I'm thinking PBKDF2 will work?)
* Public key
* TODO: fix the following
* List of the rooms that the user belongs to (encrypted to just one string with a key that is password-derived)
* Group Chats & Private Messages (encrypted to just one string with a key that is password-derived)
  * Chat ID
  * Chat key
* Privelege (either normal or admin)
  * Every user is normal by default
  * A user cannot be promoted to admin by a command given over the network, only local changes
* TODO: PMs
* TODO: See `UserData`

# Rooms
Rooms are a place where any user with an account can connect–and thus are not encrypted. Room names can only contain spaces, A-Z (only uppercase), and 0-9. A server should ignore all other requests relating to rooms that do not match the above requirements.

In addition, a server implementer may choose whether or not there are only certain "valid" rooms. Typically, any name room that a user wants can be used to send messages. If a server implementer limits rooms to only "valid" rooms, then only rooms with certain names can be used for messaging.

Room messages are not encrypted and should be stored in some random database

# Group Chats (GCs)
Any user can create a GC.
Include info about creation and management

Sending messages

Group chat messages are encrypted with a client generated key that is sent to other group members on addition or deletion of other members. 

messages stored in some random database

# Private Messages (PMs)
PM messages are encrypted with the other user's public key and are added directly to their account file on the account database
