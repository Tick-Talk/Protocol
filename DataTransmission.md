# Format
All data sent is encrypted with libsodium (with the exception being the creation of the encrypted tunnel). However, decrypted data has to be in the form of `DataType{JSON}` (a string), where:
* `DataType` is a string representing the type of data that the JSON contains (see below)
* `{JSON}` is JSON that represents the relevant data being transmitted

# Data Types and Their JSON
## Authentication & General Needs
#### `SecureTunnel`
* TBD

#### `Login`
* As this type implies, it is used for a client to login to a specific account
* Format: `{"username":"<replace>","password":"<replace>"}`

#### `UserData`
* This type represents a user's *public* data
  * See the next few entries for private information a client can use
* If `UserData` is sent from a client, the server returns the `UserData` of the user the client specifies
* Format: 

#### `SetMyData`
* Info
* Format:

#### `GetMyData`
* Info
* Format:

#### `ServerMessage`
* Info
* Format:

#### `ServerError`
* Info
* Format:

## Rooms
**AddRoom**

**DeleteRoom**

**RoomMessage**
* include replyTo JSON

**RoomFile**
* include caption JSON

**RequestRoomMessages**

## Group Chats
**MakeGroup**

**AddToGroup**

**RemoveFromGroup**

**GroupMessage**
* include replyTo JSON

#### `GroupFile`
* include caption

**RequestGroupMessages**

## PMs
**PrivateMessage**
* Info INCLUDE replyTo JSON
* Format:

**RequestPrivateMessages

#### `PrivateFile`
* Info INCLUDE CAPTION
* Format:

## Administration (can only be used by admins)
**KickUser**

**BanUser**

**DisplayServerMessage**



# OLD PROTOCOL FOR REFERENCE(DO NOT FOLLOW):
# Connecting to a chat room
`{ "type":"connect", "room":"ROOM_GOES_HERE", "name":"NAME_GOES_HERE" }`
- There are no reserved nicknames
- If a nickname is already taken, a server should send back a notice using a server message to say that
- This type of data needs to be sent from a client before any messages are sent to and from

# Server sending unencrypted errors / messages to clients
`{ "type":"server-message", "room":"ROOM_GOES_HERE", "msg":"MESSAGE_GOES_HERE" }`
- Only servers send this data type
- Server messages should not be saved by clients, and should only be displayed as a real-time error/message

# Messages
`{ "type":"message", "room":"ROOM_GOES_HERE", "timestamp":"TIME_STAMP", "name":"NAME_GOES_HERE", "msg":"MESSAGE_GOES_HERE" }`
- TIME_STAMP is a field the server calculates that represents the time in milliseconds that the message was received
- The server may only process one message per millisecond  (thus no messages can have duplicate timestamps)
- TIME_STAMP needs to be sent as a string for compatibility (otherwise the value would exceed INT_MAX)
- Messages need be destroyed one week after they are received by the server
- For messages from client to server, timestamp and name should be null (server will process both)
- A connect message needs to be sent to the server before any real-time messages are to be sent or received

# Requesting messages
`{ "type":"request", "room":"ROOM_GOES_HERE", "min":"START_TIME", "max":"END_TIME" }`
- Servers should reply with any messages within the specified range (of START_TIME to END_TIME), inclusive, using the above message protocol
- START_TIME is a string representing the lower boundary time of messages being requested
- END_TIME is a string representing the upper boundary time of messages being requested
- A user does not need to be connected to a room to request messages from that room
