Boxel-Client
============
Boxel-client is a client library for [Boxel](https://github.com/verizoncraft/boxel). 

Boxel-client allows your Minecraft plugin to connect to a Boxel server and 
draw boxelized images to an in-game screen made of blocks.

What does it do?
----------------
As described above, Boxel-client transforms data from your Boxel server into blocks
so they can be built on a minecraft server.

It handles video (at 20FPS) and websites -- you can use these basic tools to build
things like the video call and web browser from our [demos](https://verizoncraft.github.io/).

Voice, calling, ringtones and other features from the demos [here](https://verizoncraft.github.io)
are left as an exercise for the user.

(We used Websockets, WebRTC, and Redis PUB/SUB to add those capabilities)

Dependencies
-------------
In order to use Boxel-client you'll need to create your own Minecraft plugin. 
Boxel-client uses the Bukkit API and should be compatible with servers for Minecraft 1.8.

Boxel-client was developed against the open-source [GlowStone++](https://glowkitplusplus.github.io)
Minecraft server implementation, you are free to use the Bukkit compatible server of your choice. 

We recommend installing Glowkit, the Glowstone++ fork of the Bukkit API, following the instructions [here](https://github.com/GlowstonePlusPlus/Glowkit). 

Installation
------------
Once you've installed your dependencies, installing the Boxel-client library should be as simple as:

```bash
git clone git@github.com:VerizonCraft/Boxel-client.git  
cd Boxel-client  
mvn clean install  
```

Add Boxel-client to your project's dependencies and you can use it in your plugin.
For example, if you're using Maven, add the following to your pom.xml:

```XML
<dependencies>  
        <!-- you should have other dependencies... -->  
        <dependency>  
            <groupId>io.verizoncraft</groupId>  
            <artifactId>boxelclient</artifactId>  
            <version>1.0-SNAPSHOT</version>  
        </dependency> 
</dependencies>  
```

Usage
------------
In order to subscribe to streams and request renders of websites and images, you'll 
need a Boxel server to communicate with. 

You can install and run Boxel by following the instructions [here](https://github.com/verizoncraft/boxel).

Assuming you've got a Boxel server running, you can add the following to your plugin's
config.yml:

```yml
# the URI for your shared redis server
redis-uri: redis://localhost:6379/0
# the address of your boxel server's WAMP router
boxel-host: ws://localhost:8080/ws 
# the realm for your boxel service
boxel-realm: boxel  
```

Then, drawing video should be as simple as subscribing to a named stream:
```Java

public class MyPlugin extends JavaPlugin {
    private BoxelVideoClient mVideoClient;
    public void onEnable() {
      mVideoClient = new BoxelVideoClient(this);
      Location location = new Location(100, 100, 100);
      BukkitTask task = playVideo(location)
    }
    
    public BukkitTask playVideo(Location loc) {
       // stream frames at location from the channel "foo" on a 30/40 screen
       BukkitTask task = mVideoClient.subscribe(location, "foo", 30, 40);
    }
}

```

Likewise if you wanted to render a website:

```Java
   // somewhere in your plugin class 
   BoxelWebClient mWebClient = new BoxelWebClient(this); 
   // assuming you have a location and a player handy...
   mWebClient.load(loc, player, args[0], 30, 40);
```

Example Plugin
--------------
Boxel-client contains an example plugin. The easiest way to use the plugin is to
build a [shaded](https://maven.apache.org/plugins/maven-shade-plugin/) jar containing all the dependencies.

```bash
git clone git@github.com:VerizonCraft/Boxel-client.git  
cd Boxel-client  
mvn clean package  
```

Copy boxelclient-1.0-SNAPSHOT.jar from target/ to your server's plugins directory.
The example plugin provides example commands for streaming frames of video and 
rendering websites.

Browsing the source [here](https://github.com/VerizonCraft/Boxel-client/blob/master/src/main/java/io/github/verizoncraft/boxelclient/example/BoxelExamplePlugin.java) will give you a quick example of how you might use Boxel-client in your own project.

License
------------
This repository and its code are licensed under a BSD 3-Clause license, which can be found [here](https://github.com/VerizonCraft/Boxel-client/blob/master/LICENSE.txt).



