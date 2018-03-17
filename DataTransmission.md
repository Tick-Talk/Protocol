# Format
All data sent is encrypted with libsodium (with the exception being the creation of the encrypted tunnel). Data sent will be in the form of `DataType{JSON}` (a string), where:
* `DataType` is a string representing the type of data that the JSON contains (see below)
* `{JSON}` is JSON that represents the relevant data being transmitted

# Data Types and Their JSON
## Authentication
#### `PublicKey`
* Used to send the public key of the client/server
* Both the client and server send this after a connection has been established
  * No communication between a client and server can happen until all messages are encrypted with the other's public key
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>pubKey</code></td>
			<td>The public key of the sending device</td>
		</tr>
	</table>
</details>

#### `Login`
* As this type implies, it is used for a client to login to a specific account</li>
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>username</code></td>
			<td>The username of the user trying to login</td>
		</tr>
		<tr>
			<td><code>password</code></td>
			<td>The password of the user trying to login</td>
		</tr>
	</table>
</details>

## Server Necessities
#### `ServerMessage`
* This type is reserved exclusively for a server in order to broadcast important data
* It should **not** be used for anything that is not important for all users
  * A `ServerMessage` may be presented on a client as a dialogue box
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>msg</code></td>
			<td>The contents of the server message</td>
		</tr>
	</table>
</details>

#### `ServerError`
* This type is for a server to tell a client about an error (typically as a result of a client sending invalid data)
  * A `ServerError` can be displayed differently depending on its context of arrival
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>type</code></td>
			<td>The type of error that the server encountered</td>
		</tr>
		<tr>
			<td><code>description</code></td>
			<td>The description of the error that the server encountered</td>
		</tr>
	</table>
</details>

## Administration (can only be used by admins)
* To come in a later version (DO NOT IMPLEMENT):
  * KickUser
  * BanUser (for a certain amount of time)
  * DisplayServerMessage**

## User Accounts (TODO: ADD ANY OTHER FIELDS CURRENTLY MISSING)
#### `UserData`
* This type represents a user's *public* data
  * See the next few entries for private information a client can use
* If `UserData` is sent from a client, the server returns the `UserData` of the user the client specifies in the `username` field
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>username</code></td>
			<td>The username of the account in question</td>
		</tr>
		<tr>
			<td><code>picture</code></td>
			<td>The profile picture of the account in question</td>
		</tr>
		<tr>
			<td><code>pubKey</code></td>
			<td>The public key (needed for securely sending encryption keys of chats) of the account in question</td>
		</tr>
	</table>
</details>

#### `SetMyData` (TODO: SEE IF ANY OTHERS)
* This type sets the logged in user's data to whatever is specified in the JSON
* A server should only acknowledge this type if the client is already logged-in as the user in question and all fields are valid
* If anything is invalid (such as a duplicate username), a server should send a `ServerError` back
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>username</code></td>
			<td>The username of the account in question</td>
		</tr>
		<tr>
			<td><code>picture</code></td>
			<td>The profile picture of the account in question</td>
		</tr>
	</table>
</details>

#### `GetMyData` (TODO: Group chats and keys list, private messages and keys list, rooms list)
* This gets all of the needed private (encrypted) data of the user logged in
* A server should automatically send this type to a user after a successful login so a client can update itself
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>username</code></td>
			<td>The username of the account in question</td>
		</tr>
		<tr>
			<td><code>picture</code></td>
			<td>The profile picture of the account in question</td>
		</tr>
	</table>
</details>

## Rooms
#### `AddRoom`
* This type adds a room to a user's list of subscribed rooms
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>room</code></td>
			<td>The room the user is requesting to subscribe to</td>
		</tr>
	</table>
</details>

#### `DeleteRoom`
* This type deletes a room from a user's list of subscribed rooms
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>room</code></td>
			<td>The room the user is requesting to unsubscribe from</td>
		</tr>
	</table>
</details>

#### `RoomMessage`
* This type is used to send and receive messages from a particular room
  * Room messages are always *unencrypted*
  * When a message is sent from a client, all other fields except for the `msg` and `replyTo` fields are ignored by the server
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>room</code></td>
			<td>The room the user is sending a message to</td>
		</tr>
		<tr>
			<td><code>timestamp</code></td>
			<td>Also used as a message ID, the timestamp is the time in milliseconds at UTC (the server will limit messages to one a millisecond)</td>
		</tr>
		<tr>
			<td><code>replyTo</code></td>
			<td>The message ID/timestamp that `msg` is in response to (if there is one)</td>
		</tr>
		<tr>
			<td><code>msg</code></td>
			<td>The message associated with this data transmission</td>
		</tr>
	</table>
</details>

#### `RoomFile`
* This type is used to send and receive files from a particular room
  * Room files are always *unencrypted*
  * When a file is sent from a client, all other fields except for the `msg`, `replyTo`, and `caption` fields are ignored by the server
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>room</code></td>
			<td>The room the user is sending a file to</td>
		</tr>
		<tr>
			<td><code>timestamp</code></td>
			<td>Also used as a message ID, the timestamp is the time in milliseconds at UTC (the server will limit messages to one a millisecond)</td>
		</tr>
		<tr>
			<td><code>replyTo</code></td>
			<td>The message ID/timestamp that the file is in response to (if there is one)</td>
		</tr>
		<tr>
			<td><code>file</code></td>
			<td>The file encoded in Base64 that is associated with this data transmission</td>
		</tr>
		<tr>
			<td><code>caption</code></td>
			<td>The caption associated with the file</td>
		</tr>
	</table>
</details>

#### `RequestRoomMessages`
* This type is for requesting messages that were sent in a particular time frame to a room
  * Typically used for updating the client's list of messages
* When a client sends this type, a server should respond back with every `RoomMessage` and `RoomFile` sent within the requested time frame (that the server still has in memory)
* <details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>room</code></td>
			<td>The room that the client is requesting messages from</td>
		</tr>
		<tr>
			<td><code>minTime</code></td>
			<td>The timestamp that the server should start sending messages from (all messages sent as a result of this request will have a timestamp >= `minTime`)</td>
		</tr>
		<tr>
			<td><code>maxTime</code></td>
			<td>The timestamp that represents the end of the desired message range (all messages sent as a result of this request will have a timestamp <= `maxTime`)</td>
		</tr>
	</table>
</details>

## Group Chats
**MakeGroup**

**AddToGroup**

**RemoveFromGroup**
* server should check to see if nobody left, if nobody left, delete group

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
