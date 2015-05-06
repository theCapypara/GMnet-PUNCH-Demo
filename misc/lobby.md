NEW: The lobby
--------------

The newest [update to version 1.1.3](http://htme.parakoopa.de/lobbyupdate.html) introduces a new lobby system.

Fire up the demo project and let me guide you.

###First things first

Server, client and obj_control are now persistent, so they exist in the lobby room as well. There is also a new object  obj_lobbyclient, which is explained later.

###Testing the lobby

When you **start a client in the demo project and click yes when asked** you will be brought to the demo lobby room. This is a very simple demo room that can only show four servers. Once we are done, you'll be able to create an even better lobby.

To test it, start a server and see if it appears in the lobby. If you use the demo master server, please note that there might be "HTME Demo Servers" in the server list. You can not join them, they were created using the Happy Tear Multiplayer Engine PLUS (a multiplayer full engine using UDPHP) demo project. Both use the same lobby and the same demo master server.

###Data strings

There are 8 strings that can be used to identify your game in the lobby.

Open the **create** event of **obj_server** and you'll see this addition to the server creation code:
```javascript
udphp_serverSetData(1,"udphp_demo113"); //1 is reserved for game name and version! Change it when making your own game!
udphp_serverSetData(2,"UDPHP demo Server"); //2 is used for game name in our demo lobby
udphp_serverSetData(3,"This is a UDPHP demo server!"); //3 is used for game description in our demo lobby
```

These 3 strings are used by the demo project for (1) an identifier for the demo project, (2) the name of the server, (3) a description of the server. You can use all 8 for whatever you want, but (1) as a game identifier is my recommendation.

We set the data strings using ``udphp_serverSetData(n,string)`` right after the server was created. They are stored in the variables ``global.udphp_server_data1-8``.

If you want to update any data string later, after the server connected to the master server, you need to use ``udphp_serverCommitData()``. This syncs all data strings to the master server. You MUST NOT use it here, because the server hasn't connected to the master server yet!

###Building the lobby

The room of the lobby is the room ``udphhtme_lobby``. This room only contains the object ``obj_udphphtme_lobby``. This object controls the lobby. Let's dive into it!

####The create event
```javascript
//IF YOU USE UDPHP - it will only let you connect to udphp servers:
if (!script_exists(asset_get_index("htme_init"))) {
   self.game = "udphp_demo120"
}
//IF YOU USE HTME - it will only let you conect to htme servers
else {
   self.game = "htme_demo121"
}
//IF YOU USE YOUR OWN SERVER - Change self.game!

///Recieve lobby data from the master server
udphp_downloadServerList(4,"date","DESC",self.game);
```

First the variable ``game`` gets set. We use this to prevent HTME players from joining UDPHP servers and vice-versa.
As said before, this object is used in HTME PLUS demo project and UDPHP standalone demo project, this is why it has seperate code for both.

The important part here is ```[udphp_downloadServerList](http://htme.parakoopa.de/subpages/functions/udphp_downloadServerList.html)```. This will tell udphp (the part of the engine that controls communication with the master server) to download a list of servers from the master server. The paramters allow for filtering and sorting the result, we use this to only get results that match our game name we store in datastring 1 in this example. More information about what you can filter, can be found on the [usage page of udphp_downloadServerList in the HTME manual](http://htme.parakoopa.de/subpages/functions/udphp_downloadServerList.html).

You use your own filtering variables later when creating your lobby.

####The networking event

```javascript
///Waits for master server response
udphp_downloadNetworking();
```
This code checks if the master server sent the server list and updates it.

####The draw event

The draw event is split up into different sub-scripts:

#####'Background', 'Title and Controls', 'Online servers'

Draws some background colors and some text, not important

#####'Servers (Loop)'

This draws the actual server list.

Let's analyze it:

```javascript
///Servers (Loop)
var l = global.udphp_downloadlist;
for (var i = 0; i<4;i++) {
    draw_text(10,85+80*i,"=("+string(i+1)+")=");
```

First, the list ``global.udphp_downloadlist`` is stored in the local variable ``l`` (because it's shorter). **This ds_list contains all the servers we got from the master server**.

Then it begins a loop that loops through the first 4 servers in the list we got by the master server, everything is in this loop and then it draws a nice little number for each server.

```javascript
    if (ds_exists(l,ds_type_list)) {
        if (ds_list_size(l)>i) {
```

Now, this is the interesting part.

First we check if the downloadlist was already created (it get's created once the list has been downloaded). After that we check if it has at least as many entrys as the server we want to list. For this example we assume ``i`` is 1. That means it checks if there is atleast one server in the list. If yes, we have an entry we can now draw.

```javascript
            //Get stuff from the downloadlist
            var entry = l[| i];
            var ip = entry[? "ip"];
            var game = entry[? "data1"];
            var servername = entry[? "data2"];
            var description = entry[? "data3"];
```

Now the entry (a ds_map) for our server is extracted from the list and we get the gamename, which is stored in data1, the ip, which is stored in the key "ip", the name of the server, which we stored in data2, and so on.

```javascript
            draw_text(70,85+80*i,servername+" | "+ip);
            draw_text(70,115+80*i,description);
        }
    }
    draw_line(0,160+80*i,room_width,160+80*i);
}
```

Now we just draw everything.

#####'Footer'

Again, just some text, not important.

#### The press 1-4 key events

Pressing 1-4 on the keyboard will connect to that game. Let's see how!

```javascript
///LOAD GAME SERVER ON SLOT 1
var l = global.udphp_downloadlist;
if (ds_exists(l,ds_type_list)) {
    if (ds_list_size(l)>0) {
        var entry = l[| 0];
        var ip = entry[? "ip"];
        var game = entry[? "data1"];
```

We again open the downloadlist and check if server 1 is in it, if yes we continue.

```javascript
        if (game != self.game) {
           //Not compatible game, exit
           show_message("Game server or version is incompatible!");
           exit;
        }
```

Remember the filtering variable we created in the create-event? We use it here to check if the server is a udphp demo game. If not we cancel.
Please note, that this is propably not needed here, since we filtered out all, but our game in the create event, when we ran udphp_downloadServerList.

```javascript
        //====UDPHP DEMO ONLY
        if (!script_exists(asset_get_index("htme_init"))) {
            //Create new client - See obj_client + manual for more information
            global.tmp_lobby_ip = ip;
            instance_create(0,0,asset_get_index("obj_lobbyclient"));
            //Return to main room
            room_goto(asset_get_index("udphp_room"));
        }
        //====HTME DEMO ONLY
        else {
            //This code is irrelevant for UDPHP and has been removed
        }
    } else {
      //Do nothing - There is no server on this slot
    }
} else {
  //Do nothing - There is no server on this slot
}
```

This is the rest of the script. We once again check if we are running the HTME demo project and then we begin connection.

We create a new instance of obj_lobbyclient, a object that is child of the object obj_client.This means it has the same events. We only changed the create event to get the ip from ``global.tmp_lobby_ip`` rather than asking for it. It will create a new client same way ``obj_client`` does and control it.

Done!

And this is how you create a lobby! Now go ahead and do it! :)

###How can my clients get the gamename and data strings after they connected?

Please see script ``udphp_clientReadData`` for how this works.

###Anything missing?

If this bonus chapter lacks something important, please let me know. If you have any problems feel free to contact me.
