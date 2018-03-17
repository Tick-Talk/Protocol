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
* <details>
<summary>Format</summary>
Field | Value
----- | -----
username | The username of the user trying to login
password | The password of the user trying to login
</details>

#### `UserData`
* This type represents a user's *public* data
  * See the next few entries for private information a client can use
* If `UserData` is sent from a client, the server returns the `UserData` of the user the client specifies
* Format: `{"username":""}`

#### `SetMyData`
* This type sets the logged in user's data to whatever is specified in the JSON
* A server should only acknowledge this type if the client is already logged-in as the user in question and all fields are valid
* If anything is invalid (such as a duplicate username), a server should send a `ServerError` back
* Format: `{"username":"<replace--this field is to change username>","picture":"<replace--Base64 of picture>"}`
* TODO: add any other things to this

#### `GetMyData`
* This gets all of the needed private data of the user logged in
* A server should automatically send this type to a user after a successful login so a client can update itself
* Format: `{"username":}`

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
