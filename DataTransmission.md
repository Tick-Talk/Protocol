# Format
In order to simplify sending and receiving data, the Tick Talk protocol, as of v6.0.0, is built on top of [Requests](https://github.com/GregoryConrad/Requests).
Requests features two different independent fields that are utilized in Tick Talk: `name` and `data`.
A `name` is analagous to a function/method name in programming and `data` is analagous to a list of parameters or return values. The following section describes what `name`s are valid and what `data` is valid along with them.

# Names and their data formats
## Server Stuff
#### `displayServerMessage` (1 way)
* Purpose: To show a server message (can only be sent by a server)
* Parameter: The server message, as a string, to display
* Note(s): Server messages can be displayed as alert/dialogue boxes or something similar

## User Accounts
#### `login` (2 way)
* Purpose: Sent from a client to login a user
* Parameter: A [`LoginObject`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#loginobject)
* Return: The signed in user's [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata)
* Error: The error encountered as a string

#### `getUserData` (2 way)
* Purpose: A request to get the [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object of a user
* Parameter: The username of a user
* Return: The [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object of the requested user, with blank strings populating the private fields
* Error: The error encountered (typically user not found) as a string

#### `updateUserData` (1 way)
* Purpose: To give a client an updated [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object when something happens externally, like if they are removed from a group
* Parameter: A [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object

#### `setMyPicture` (2 way)
* Purpose: To set the logged-in user's picture
* Parameter: The base-64 representation of the contents of a PNG file
* Return: The updated [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object of the signed in user
* Error: The error encountered as a string

#### `setMyDisplayName` (2 way)
* Purpose: To set the logged-in user's display name
* Parameter: The new display name
* Return: The updated [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object of the signed in user
* Error: The error encountered as a string

## Rooms
#### `sendRoomMessage` (1 way)
* Purpose: To send a message to a room
* Parameter: A [`RoomMessage`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#roommessage) object with all fields except for the `id` and `timestamp` fields populated
* Notes: Room messages are *unencrypted*

#### `showRoomMessage` (1 way)
* Purpose: To tell a client there is a [`RoomMessage`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#roommessage) that a client doesn't have (either because the message is new or because it was requested by the client)
* Parameter: The [`RoomMessage`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#roommessage) object
* Notes: THIS IS ONLY FROM A SERVER TO A CLIENT

#### `getRoomMessageBlock` (1 way)
* Purpose: To get a block (a set) of ~1000 room messages from the server, for when a user exhausts the client's cache (if there is any at all)
* Parameter: A [`MessageBlockRequest`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#messageblockrequest) object

#### `getNewRoomMessages` (1 way)
* Purpose: To get any new messages sent after a client has been offline for some time
* Parameter: A [`NewMessageRequest`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#newmessagerequest) object

## Group Chats (& PMs)
#### `makeGroup` (2 way)
* Purpose: To create a new group chat
* Parameter: A [`MakeGroup`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#makegroup) object
* Return: A blank string (getting a return indicates success)
* Error: The error encountered as a string

#### `changeGroupInfo` (2 way)
* Purpose: To change some data about a group, like its description or picture
* Parameter: A [`ChangeGroupInfo`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#changegroupinfo) object
* Return: An updated [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object
* Error: The error encountered as a string

#### `changeGroupUser` (2 way)
* Purpose: To add, remove, or change the nickname of a user in a group
* Parameter: A [`ChangeGroupUser`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#changegroupuser) object
* Return: An updated [`UserData`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#userdata) object
* Error: The error encountered as a string

#### `sendGroupMessage` (1 way)
* Purpose: To send a message to a group
* Parameter: A [`GroupMessage`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#groupmessage) object with all fields except for the `id` and `timestamp` fields populated
* Notes: Group messages are *encrypted*

#### `showGroupMessage` (1 way)
* Purpose: To tell a client there is a [`GroupMessage`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#groupmessage) that a client doesn't have (either because the message is new or because it was requested by the client)
* Parameter: The [`GroupMessage`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#groupmessage) object
* Notes: THIS IS ONLY FROM A SERVER TO A CLIENT

#### `getGroupMessageBlock` (1 way)
* Purpose: To get a block (a set) of ~1000 group messages from the server, for when a user exhausts the client's cache (if there is any at all)
* Parameter: A [`MessageBlockRequest`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#messageblockrequest) object

#### `getNewGroupMessages` (1 way)
* Purpose: To get any new messages sent after a client has been offline for some time
* Parameter: A [`NewMessageRequest`](https://github.com/Tick-Talk/Protocol/blob/master/JSON-Objects.md#newmessagerequest) object
