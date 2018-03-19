# Tick Talk Protocol
### Current Protocol Version: v0.0.0-alpha
------------------------------------------
**Created by Gregory Conrad in March 2018**

TODO: Describe Tick Talk

For more information on how Tick Talk operates, open up the corresponding file of your query in this repository.

https://github.com/tchapi/markdown-cheatsheet

Notes to me:

Change up protocol so that whenever a change occurs that would affect a user, a server sends all updated user data to the client instead of server sending back same type which is dumb (does not include private messages--this is seperate than user data, even though it will be stored with it on the server side. it has to be requested seperately)

* On login: server sends all data (not PMs)
* On any change to rooms and shiz: server sends updated data (again, no PMs)
* etc.
