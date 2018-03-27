# Considerations During Implementation
Server creators have to keep many different security considerations in mind when implementing a server, for obvious reasons. When implementing a `DataType` in DataTransmission.md, a developer should always think:
* "Does this user have adequate priveleges to do the action they are requesting?"
* "Which logged-in users need to be sent a new `UserData` as a result of this `DataType` being executed?"
* "How can I make this as secure as possible?"
Look into https://github.com/novnc/websockify as a potential method to run the server. Looks promising.

# Accounts
### Account Creation and Deletion
Account creation will happen on a webserver which has access to the account database. A simple PHP page will function to create, delete, and manage accounts. This is technically a part of the server that will be implemented. It is vital that the webserver uses (secure) SSL/HTTPS--this part is up to a server owner.

### Account Database
Accounts can be stored in any way desired on the server. However, a SQL database is recommended and desired.

Each user account needs to be stored with the data that can be found in `UserData` in DataTransmission.md.
In addition, each user needs to be stored with the following:
* Privelege (either normal or admin)
  * Every user is normal by default
  * A user can *only* be promoted to admin by a local change to ensure security
* Suggestion: Private Messages
  * Private messages do not have to be stored along with a user account, but it would be a good idea for simplicity
  * In order to store them, a hashmap of string usernames to a list of messages would be wise

# Rooms
Rooms are a place where any user with an account can connect–and thus are not encrypted. Room names can only be alphanumeric with spaces. A server should not initialize any rooms that do not meet the above requirements.

Room messages are not encrypted and should be stored in a database dedicated to room messaging.

# Group Chats (GCs)
Any user can create a GC. See DataTransmission.md for how.

Group chat messages are encrypted with a server-generated key (that is immediately securely erased after usage) that is sent to group members on group creation and removal of members.

Group chat messages should be stored in a database dedicated to just group chat messaging.

# Private Messages (PMs)
Private messages are simply user to user messages. They are encrypted with the other user's public key and are saved alongside a user's account data (see the "Account Database" section above)
