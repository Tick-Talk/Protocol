# Connecting to the server
In order to send and receive messages, a client must first connect to at least one chatroom.

# Connecting to a chat room
`{ "type":"connect", "room":"<ROOM_GOES_HERE>", "name":"<NAME_GOES_HERE>" }`

# Disconnecting from a chat room
`{ "type":"disconnect", "room":"<ROOM_GOES_HERE>" }`

# Messages
`{ "type":"message", "room":"<ROOM_GOES_HERE>", "id":<ID_NUM_GOES_HERE>, "name":"<NAME_GOES_HERE>", "msg":"<MESSAGE_GOES_HERE>" }`
- The nickname "SERVER" is dedicated for the server, which can be used for errors
- Servers should send an error back if the nickname is already taken
- The ID is the number that the server creates and represents a message (0 <= ID <= 999,999,999); if 1 billion is reached, ID should be reset to 0
- Servers should drop a message one week after receiving
- Clients should drop a message one week after receiving
- For messages from client to server, ID and name should be null

# Requesting messages
`{ "type":"request", "min":<MIN_ID_REQUESTING>, "max":<MAX_ID_REQUESTING> }`
- Servers should reply with any messages within the specified range
