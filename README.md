# General Rules
1. The first character of any data sent signifies what type of data is being sent (this will be referred to as the "signifier" below)
2. The actual data being sent is JSON, which is appended to the end of the signifier (described in the next section for each specific situation)
3. Anything that is malformed or is in any other way invalid for the context should be ignored (unless an explicit exception is defined in this protocol)
4. Chatroom names must be at least one character long. If a chatroom name is blank or null, it should be treated as the chatroom "main" (without quotes)
5. Nicknames must be at least one character long, and cannot be "SERVER" (without quotes). If a user has a blank nickname for a chatroom, simply consider the user as not apart of the chatroom as per rule 3
6. Messages must be dropped after one week (+/- a day). (One week is 604800 seconds)
7. Encryption is left up to the client and server developer(s) to decide upon

# Each type of valid data  
** Messages **  
- Messages must have the signifier 'M' (the capital letter).
- *For clients sending messages to servers*
- The data's JSON format: `{"CHATROOM":"Chatroom goes here", "MESSAGE": "Message goes here"}`
- Complete example of a valid message being sent from a client to a server: `M{"CHATROOM":"main", "MESSAGE": "My message"}`
- *For servers sending messages to clients*
- The nickname "SERVER" is reserved for when a server needs to tell a client something. Server messages must be decided upon between the developer(s)
- The main difference here is that when a server sends a message to a client, it must also include the nickname of the person who sent the message
- The data's JSON format: `{"CHATROOM":"Chatroom goes here", "NICKNAME":"Nickname goes here", "MESSAGE": "Message goes here"}`
- Complete example of a valid message being sent from a server to a client: `M{"CHATROOM":"main", "NICKNAME":"ja boi", "MESSAGE": "hello"}`

** Connected Chatroom List **
- Chatroom lists must have the signifier 'C' (the captial letter)
- A chatroom list must be sent to a server BEFORE any messages are sent from the client
- If a user sends a message to a chatroom they aren't apart of, a server should ignore the message. A client must send their ENTIRE chatroom list whenever it changes (or on connection to the server)
- A server should make sure there are no duplicate nicknames. If a user tries to use a duplicate nickname, a server should send an error message back using the "SERVER" nickname.
- A nickname must be set for each chatroom (see the below JSON format)
- Servers should have a limit for how often a user can change their nickname for a specific chatroom, but this is not required.
- The data's JSON format: (see https://jsonlint.com/?json=%7B%0D%0A%09%22LIST%22%3A%5B%7B%0D%0A%09%09%09%22CHATROOM%22%3A%22Chatroom%20goes%20here%22%2C%0D%0A%09%09%09%22NICKNAME%22%3A%22Nickname%20goes%20here%22%0D%0A%09%09%7D%2C%0D%0A%09%09%7B%0D%0A%09%09%09%22CHATROOM%22%3A%22Chatroom%202%20goes%20here%22%2C%0D%0A%09%09%09%22NICKNAME%22%3A%22Nickname%202%20goes%20here%22%0D%0A%09%09%7D%0D%0A%09%5D%0D%0A%7D)


The following will come soon:
- Requests for messages and getting the requested messages (this will probably be in a future release)
