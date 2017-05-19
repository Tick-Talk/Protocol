# Protocol
A protocol for the server and clients to communicate

# General Rules
1. The first character of any data sent signifies what type of data is being sent (this will be referred to as the "signifier" below)
2. The actual data being sent is JSON, which is appended to the end of the signifier (described in the next section for each specific situation)
3. Anything that is malformed or is in any other way invalid for the context should be ignored (unless an explicit exception is defined in this protocol)
4. Chatroom names must be at least one character long. If a chatroom name is blank or null, it should be treated as the chatroom "main" (without quotes)
5. Nicknames must be at least one character long, and cannot be "SERVER" (without quotes). If a user has a blank nickname for a chatroom, simply consider the user as not apart of the chatroom as per rule 3
6. Messages must be dropped after one week (+/- a day). (One week is 604800 seconds)
7. Encryption is left up to the client and server developer(s) to decide upon

# Each type of valid data  
*** Messages ***  
- Messages must have the signifier 'M' (the capital letter).
- *For clients sending messages to servers*
- The data's JSON format: `{"CHATROOM":"Chatroom goes here", "MESSAGE": "Message goes here"}`
- Complete example of a valid message being sent from a client to a server: `M{"CHATROOM":"main", "MESSAGE": "My message"}`
- *For servers sending messages to clients*
- The nickname "SERVER" is reserved for when a server needs to tell a client something. Server messages must be decided upon between the developer(s)
- The main difference here is that when a server sends a message to a client, it must also include the nickname of the person who sent the message
- The data's JSON format: `{"CHATROOM":"Chatroom goes here", "NICKNAME":"Nickname goes here", "MESSAGE": "Message goes here"}`
- Complete example of a valid message being sent from a server to a client: `M{"CHATROOM":"main", "NICKNAME":"ja boi", "MESSAGE": "hello"}`

The following will come soon:
- Chatroom / nickname list (should have a time limit for how often changes can occur, no duplicate nicks)  
- Request for messages
