This file describes JSON objects that can be sent as parameters or return values.

Please note: any data sent in JSON form must not contain NULL.

#### `LoginObject`
* As of v6.1.0, this object includes two arrays of arrays that specify the last received message of groups and rooms
  * A server should respond with all new messages sent since that last received message
<details>
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
		<tr>
			<td><code>protocol</code></td>
			<td>The *major* protocol version as an int to check for compatibility</td>
		</tr>
		<tr>
			<td><code>lastSeenGroupMsgIDs</code></td>
			<td>An array of arrays, with index 0 being the groupID and index 1 being the last seen msg ID</td>
		</tr>
		<tr>
			<td><code>lastSeenRoomMsgIDs</code></td>
			<td>An array of arrays, with index 0 being the room and index 1 being the last seen msg ID</td>
		</tr>
	</table>
</details>

#### `UserData`
* This type represents a user's data
  * It is commonly sent from a server to a client in order to refresh the client's cache and UI
* `UserData` will contain the private fields *if and only if* the `UserData` being requested is for the logged-in user
  * Otherwise, they should be blank strings
<details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>username</code></td>
			<td>The username of the account in question</td>
		</tr>
		<tr>
			<td><code>displayName</code></td>
			<td>The display name for the account in question</td>
		</tr>
		<tr>
			<td><code>picture</code></td>
			<td>The Base64 of the profile picture of the account in question</td>
		</tr>
		<tr>
			<td><code>pubKey</code></td>
			<td>The public key of the account in question</td>
		</tr>
		<tr>
			<td><code>privateKey</code></td>
			<td>(PRIVATE FIELD) The (encrypted with key-derived password) private key of the user</td>
		</tr>
		<tr>
			<td><code>rooms</code></td>
			<td>(PRIVATE FIELD) A list of the rooms the user can access (a JSON array of strings)</td>
		</tr>
		<tr>
			<td><code>groups</code></td>
			<td>
(PRIVATE FIELD) A list of JSON dictionaries that have the following fields:
				
* `groupID`: The ID of the group in question
* `name`: The name of the group
* `pic`: The profile picture of the group
* `description`: The description of the group
* `nicknames`: A JSON map of usernames to their corresponding nicknames
			</td>
		</tr>
	</table>
</details>

#### `RoomMessage`
* When sending a file, the `msg` field represents the file's caption
<details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>room</code></td>
			<td>The room the user is sending a message to</td>
		</tr>
		<tr>
			<td><code>id</code></td>
			<td>The alphanumeric ID of the message that allows it to be sorted amongst other messages</td>
		</tr>
		<tr>
			<td><code>timestamp</code></td>
			<td>The timestamp is the time in seconds at UTC (the server generates this field)</td>
		</tr>
		<tr>
			<td><code>replyTo</code></td>
			<td>The message ID that `msg` is in response to (if there is one)</td>
		</tr>
		<tr>
			<td><code>msg</code></td>
			<td>The message (or caption) associated with this data transmission</td>
		</tr>
		<tr>
		    <td><code>file</code></td>
		    <td>The file encoded in Base64 that is associated with this data transmission</td>
		</tr>
		<tr>
		    <td><code>filename</code></td>
		    <td>The name of the file</td>
		</tr>
	</table>
</details>

#### `MessageBlockRequest`
<details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>id</code></td>
			<td>The room/groupID that the client is requesting messages from</td>
		</tr>
		<tr>
			<td><code>lastMessageID</code></td>
			<td>The last message, inclusive, that is in the block of messages. Blank string ("") for the most recent block of messages</td>
		</tr>
	</table>
</details>

#### `MakeGroup`
* This type is sent to a server from a client to create a new group chat
* Upon receiving the request, a server should create the group and add the user who requested the creation of the group and any users they specify to the group
<details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>groupName</code></td>
			<td>The desired name for this group chat</td>
		</tr>
		<tr>
			<td><code>usernames</code></td>
			<td>An array of usernames that specifies the users to be added to this group</td>
		</tr>
	</table>
</details>

#### `ChangeGroupInfo`
* This type is used to set/change anything relating to a group
  * The group picture, description, or name can be changed with this data format
<details>
  	<summary>Format</summary>
  	<table>
  		<tr><th>Field</th><th>Value</th></tr>
  		<tr>
  		    <td><code>groupID</code></td>
  		    <td>The ID of the group in question</td>
  		</tr>
  		<tr>
  		    <td><code>whatToChange</code></td>
  		    <td>As the name implies, this field specifies what is being requested to change (either <code>name</code>, <code>description</code> or <code>pic</code>)</td>
  		</tr>
  		<tr>
          	    <td><code>data</code></td>
          	    <td>The data relating to what is specified in <code>whatToChange</code></td>
       	        </tr>
  	</table>
</details>
  
#### `ChangeGroupUser`
* This type is used to perform maintenance on a user in a group
  * Can be used to add a user to a group, remove a user from a group, or change the nickname of a user in a group
  * If removing a user, a server should delete a group if nobody is left
<details>
  	<summary>Format</summary>
  	<table>
  		<tr><th>Field</th><th>Value</th></tr>
  		<tr>
  			<td><code>groupID</code></td>
  			<td>The ID of the group in question</td>
  		</tr>
  		<tr>
          	<td><code>mode</code></td>
        	<td>What is happening to the user specified in <code>username</code>(either <code>add</code>, <code>remove</code>, or <code>nickname</code>)</td>
        </tr>
  		<tr>
  			<td><code>username</code></td>
  			<td>The username of the user in question</td>
  		</tr>
  		<tr>
          	<td><code>nickname</code></td>
          	<td>The new nickname for the user in question (if applicable)</td>
        </tr>
  	</table>
</details>
 
#### `GroupMessage`
* This type is used to send and receive messages from a group chat
  * Group chat messages are always encrypted
* When a message is sent from a client, the `timestamp` field is ignored by the server (as this is a server-generated field)
* When a file is sent, the `msg` field is used for the file's caption
* ALL JSON maps must include data for the user sending the message too
<details>
	<summary>Format</summary>
	<table>
		<tr><th>Field</th><th>Value</th></tr>
		<tr>
			<td><code>groupID</code></td>
			<td>The ID of the group that the user is sending a message to</td>
		</tr>
		<tr>
			<td><code>id</code></td>
			<td>The alphanumeric ID of the message that allows it to be sorted amongst other messages</td>
		</tr>
		<tr>
			<td><code>timestamp</code></td>
			<td>The timestamp is the time in seconds at UTC (the server generates this field)</td>
		</tr>
		<tr>
			<td><code>replyTo</code></td>
			<td>The message ID/timestamp that `msg` is in response to (if there is one)</td>
		</tr>
		<tr>
			<td><code>encryptedMsgData</code></td>
			<td>
				This field is for an array of arrays. Each sub-array represents the encrypted data for one invididual user. If something cannot be included in the array/is not relevant, it should be a blank string. A sub-array needs to be present for every user involved in communication--including the sender. Each sub array should be in the following format:
* [0]: `username` - the username of the user which the following data is encrypted for
* [1]: `msg` - the encrypted message for the user
* [2]: `msgNonce` - the nonce used to encrypt the message for the user
* [3]: `filename` - the encrypted filename for the user
* [4]: `filenameNonce` - the nonce used to encrypt the filename for the user
* [5]: `file` - the encrypted form of the Base64 representation of a file for a user
* [6]: `fileNonce` - the nonce used to encrypt the Base64 representation of a file for a user
			</td>
		</tr>
	</table>
</details>
