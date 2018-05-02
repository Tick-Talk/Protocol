# Considerations During Implementation
Server creators have to keep many different security considerations in mind when implementing a server, for obvious reasons. When implementing a `DataType` in DataTransmission.md, a developer should always think:
* "Does this user have adequate priveleges to do the action they are requesting?"
* "Which logged-in users need to be sent a new `UserData` as a result of this `DataType` being executed?"
* "How can I make this as secure as possible?"

# Accounts
### Account Creation and Deletion
TODO

### Account Database
Accounts can be stored in any way desired on the server. However, a SQL database is recommended and desired.

Each user account needs to be stored with the data that can be found in `UserData` in DataTransmission.md.
In addition, each user needs to be stored with the following:
* Privelege (either normal or admin)
  * Every user is normal by default
  * A user can *only* be promoted to admin by a local change to ensure security

# Rooms
Rooms are a place where any user with an account can connect–and thus are not encrypted. Room names can only be alphanumeric with spaces. A server should not initialize any rooms that do not meet the above requirements.

Room messages are not encrypted and should be stored in a database dedicated to room messaging.

# Group Chats (GCs)
Any user can create a GC. See DataTransmission.md for how.

Group chat messages are encrypted (by the sender) for each user with their respective public key and a secure nonce.

How storage of group chat messages should work is up to the server implementer, as it can be highly dependent on how the server is constructed.

# Private Messages (PMs)
As of protocol v2.0.0, use GCs for all private messaging needs.
