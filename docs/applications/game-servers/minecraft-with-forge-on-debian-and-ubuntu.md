---
author:
   name: Linode Community
   email: docs@linode.com
description: 'Minecraft with Forge installation guide for Debian and Ubuntu'
keywords: 'minecraft,forge,mod,modded,dedicated,server,ubuntu,debian'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
external_resources:
 - '[Minecraft.net](https://minecraft.net/)'
 - '[MinecraftForge.net](http://www.minecraftforge.net/)'
modified: '-'
modified_by:
   name: Linode
published: '-'
title: 'Minecraft with Forge Dedicated Server on Debian and Ubuntu'
contributor:
   name: Davide Beatrici
   link: https://github.com/davidebeatrici
---

*This is a Linode Community guide. [Write for us](/docs/contribute) and earn $250 per published guide.*

<hr>

This guide will show you how to install your own Minecraft server with Forge on a Linode running Debian or Ubuntu.

##Prerequisites

1. Check for updates:

        sudo apt-get update && sudo apt-get upgrade

2. Install **OpenJDK**, an open-source implementation of Java:

        sudo apt-get install openjdk-7-jre-headless

3. Create a new user for Minecraft to run as. Never run as root, for security:
        
        useradd -m minecraft

{:.note}
>
> If you have a firewall configured according to our [Securing Your Server](/docs/security/securing-your-server) guide, you will need to add an exception for the port 25565. The line to add to your `iptables.firewall.rules` file is:
>
>     -A INPUT -p tcp --dport 25565 -j ACCEPT

##Installing Minecraft with Forge server

1. Switch to your newly created account:

        sudo -u minecraft -i

2. Create a folder for Minecraft Forge server:

        mkdir MinecraftForge

3. Go into your newly created folder:

        cd MinecraftForge

4. Download Minecraft Forge:

        wget http://files.minecraftforge.net/maven/net/minecraftforge/forge/1.8.9-11.15.0.1672/forge-1.8.9-11.15.0.1672-installer.jar

      {:.note}
      >
      > This URL costantly changes as Forge is updated. Please check the downloads [page](http://files.minecraftforge.net/) for the current URL.

5. Execute the installer:

        java -jar forge-1.8.9-11.15.0.1672-installer.jar nogui --installServer

6. Cleanup:

        rm forge-1.8.9-11.15.0.1672-installer.jar
        rm forge-1.8.9-11.15.0.1672-installer.jar.log

7. Create a script to run the server:

    {: .file }
    /home/minecraft/MinecraftForge/run.sh
    :   ~~~ sh
        #!/bin/sh
        BINDIR=$(dirname "$(readlink -fn "$0")")
        cd "$BINDIR"

        java -Xms512M -Xmx1000M -jar forge-1.8.9-11.15.0.1672-universal.jar -o true
        ~~~

    {: .note }
    > The `Xms` and `Xmx` flags define the minimum and maximum amount of RAM the Minecraft server will use. The settings above are          recommended for a Linode 1GB used solely for this purpose. Adjust these values to fit your needs.

4. Make `run.sh` executable:

        chmod +x run.sh

##Running Minecraft with Forge server

1. Execute the run script:

        $ ./run.sh
        [00:54:08] [main/INFO] [LaunchWrapper]: Loading tweak class name net.minecraftforge.fml.common.launcher.FMLServerTweaker
        [00:54:08] [main/INFO] [LaunchWrapper]: Using primary tweak class name net.minecraftforge.fml.common.launcher.FMLServerTweaker
        [00:54:08] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.common.launcher.FMLServerTweaker
        [00:54:10] [main/INFO] [FML]: Forge Mod Loader version 11.15.0.1672 for Minecraft 1.8.9 loading
        [00:54:10] [main/INFO] [FML]: Java is OpenJDK 64-Bit Server VM, version 1.7.0_91, running on
        Linux:amd64:4.1.5-x86_64-linode61, installed at /usr/lib/jvm/java-7-openjdk-amd64/jre
        [00:54:10] [main/INFO] [LaunchWrapper]: Loading tweak class name
        net.minecraftforge.fml.common.launcher.FMLInjectionAndSortingTweaker
        [00:54:10] [main/INFO] [LaunchWrapper]: Loading tweak class name net.minecraftforge.fml.common.launcher.FMLDeobfTweaker
        [00:54:10] [main/INFO] [LaunchWrapper]: Calling tweak class
        net.minecraftforge.fml.common.launcher.FMLInjectionAndSortingTweaker
        [00:54:10] [main/INFO] [LaunchWrapper]: Calling tweak class
        net.minecraftforge.fml.common.launcher.FMLInjectionAndSortingTweaker
        [00:54:10] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.relauncher.CoreModManager$FMLPluginWrapper
        [00:54:13] [main/ERROR] [FML]: FML appears to be missing any signature data. This is not a good thing
        [00:54:13] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.relauncher.CoreModManager$FMLPluginWrapper
        [00:54:13] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.common.launcher.FMLDeobfTweaker
        [00:54:15] [main/INFO] [LaunchWrapper]: Loading tweak class name net.minecraftforge.fml.common.launcher.TerminalTweaker
        [00:54:15] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.common.launcher.TerminalTweaker
        [00:54:17] [main/INFO] [LaunchWrapper]: Launching wrapped minecraft {net.minecraft.server.MinecraftServer}
        [00:54:27] [Server thread/INFO]: Starting minecraft server version 1.8.9
        > [00:54:27] [Server thread/INFO] [FML]: MinecraftForge v11.15.0.1672 Initialized
        [00:54:27] [Server thread/INFO] [FML]: Replaced 204 ore recipies
        [00:54:27] [Server thread/INFO] [FML]: Config directory created successfully
        [00:54:28] [Server thread/INFO] [FML]: Found 0 mods from the command line. Injecting into mod discoverer
        [00:54:28] [Server thread/INFO] [FML]: Searching /home/minecraft/MinecraftForge/mods for mods
        [00:54:29] [Server thread/INFO] [FML]: Forge Mod Loader has identified 3 mods to load
        [00:54:30] [Server thread/INFO] [FML]: Attempting connection with missing mods [mcp, FML, Forge] at CLIENT
        [00:54:30] [Server thread/INFO] [FML]: Attempting connection with missing mods [mcp, FML, Forge] at SERVER
        [00:54:31] [Server thread/INFO] [FML]: Processing ObjectHolder annotations
        [00:54:31] [Server thread/INFO] [FML]: Found 384 ObjectHolder annotations
        [00:54:31] [Server thread/INFO] [FML]: Identifying ItemStackHolder annotations
        [00:54:31] [Server thread/INFO] [FML]: Found 0 ItemStackHolder annotations
        [00:54:31] [Server thread/INFO] [FML]: Configured a dormant chunk cache size of 0
        [00:54:31] [Server thread/INFO] [FML]: Applying holder lookups
        [00:54:31] [Forge Version Check/INFO] [ForgeVersionCheck]: [Forge] Starting version check at
        http://files.minecraftforge.net/maven/net/minecraftforge/forge/promotions_slim.json
        [00:54:31] [Server thread/INFO] [FML]: Holder lookups applied
        [00:54:31] [Server thread/INFO] [FML]: Injecting itemstacks
        [00:54:31] [Server thread/INFO] [FML]: Itemstack injection complete
        [00:54:31] [Server thread/INFO]: Loading properties
        [00:54:31] [Server thread/WARN]: server.properties does not exist
        [00:54:31] [Server thread/INFO]: Generating new properties file
        [00:54:31] [Server thread/WARN]: Failed to load eula.txt
        [00:54:31] [Server thread/INFO]: You need to agree to the EULA in order to run the server. Go to eula.txt for more info.
        [00:54:31] [Server thread/WARN] [FML]: Can't revert to frozen GameData state without freezing first.
        [00:54:31] [Server thread/INFO] [FML]: The state engine was in incorrect state POSTINITIALIZATION and forced into state
        SERVER_STOPPED. Errors may have been discarded.

2. The first time you run the Minecraft server it will create an EULA file and then exit. Open the `eula.txt` file and change the value of `eula` to true:

   {: .file }
   /home/minecraft/eula.txt
    :   ~~~ sh
        #By changing the setting below to TRUE you are indicating your agreement to our EULA        
        (https://account.mojang.com/documents/minecraft_eula).
        #Thu Jan 01 00:00:00 UTC 1970
        eula=true
        ~~~

3. To ensure that the Minecraft server runs, dependent of an SSH connection, execute `run.sh` from within a [GNU Screen](/docs/networking/ssh/using-gnu-screen-to-manage-persistent-terminal-sessions) session:

        screen /home/minecraft/run.sh

    This time the Minecraft server console will generate a lot of output as it creates required text files and generates the Minecraft world:

        [01:08:43] [main/INFO] [LaunchWrapper]: Loading tweak class name net.minecraftforge.fml.common.launcher.FMLServerTweaker
        [01:08:43] [main/INFO] [LaunchWrapper]: Using primary tweak class name net.minecraftforge.fml.common.launcher.FMLServerTweaker
        [01:08:43] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.common.launcher.FMLServerTweaker
        [01:08:45] [main/INFO] [FML]: Forge Mod Loader version 11.15.0.1672 for Minecraft 1.8.9 loading
        [01:08:45] [main/INFO] [FML]: Java is OpenJDK 64-Bit Server VM, version 1.7.0_91, running on
        Linux:amd64:4.1.5-x86_64-linode61, installed at /usr/lib/jvm/java-7-openjdk-amd64/jre
        [01:08:45] [main/INFO] [LaunchWrapper]: Loading tweak class name
        net.minecraftforge.fml.common.launcher.FMLInjectionAndSortingTweaker
        [01:08:45] [main/INFO] [LaunchWrapper]: Loading tweak class name net.minecraftforge.fml.common.launcher.FMLDeobfTweaker
        [01:08:45] [main/INFO] [LaunchWrapper]: Calling tweak class
        net.minecraftforge.fml.common.launcher.FMLInjectionAndSortingTweaker
        [01:08:45] [main/INFO] [LaunchWrapper]: Calling tweak class
        net.minecraftforge.fml.common.launcher.FMLInjectionAndSortingTweaker
        [01:08:45] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.relauncher.CoreModManager$FMLPluginWrapper
        [01:08:47] [main/ERROR] [FML]: FML appears to be missing any signature data. This is not a good thing
        [01:08:47] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.relauncher.CoreModManager$FMLPluginWrapper
        [01:08:47] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.common.launcher.FMLDeobfTweaker
        [01:08:49] [main/INFO] [LaunchWrapper]: Loading tweak class name net.minecraftforge.fml.common.launcher.TerminalTweaker
        [01:08:49] [main/INFO] [LaunchWrapper]: Calling tweak class net.minecraftforge.fml.common.launcher.TerminalTweaker
        [01:08:52] [main/INFO] [LaunchWrapper]: Launching wrapped minecraft {net.minecraft.server.MinecraftServer}
        [01:09:02] [Server thread/INFO]: Starting minecraft server version 1.8.9
        [01:09:02] [Server thread/INFO] [FML]: MinecraftForge v11.15.0.1672 Initialized
        [01:09:02] [Server thread/INFO] [FML]: Replaced 204 ore recipies
        [01:09:03] [Server thread/INFO] [FML]: Found 0 mods from the command line. Injecting into mod discoverer
        [01:09:03] [Server thread/INFO] [FML]: Searching /home/minecraft/MinecraftForge/mods for mods
        [01:09:04] [Server thread/INFO] [FML]: Forge Mod Loader has identified 3 mods to load
        [01:09:05] [Server thread/INFO] [FML]: Attempting connection with missing mods [mcp, FML, Forge] at CLIENT
        [01:09:05] [Server thread/INFO] [FML]: Attempting connection with missing mods [mcp, FML, Forge] at SERVER
        [01:09:06] [Server thread/INFO] [FML]: Processing ObjectHolder annotations
        [01:09:06] [Server thread/INFO] [FML]: Found 384 ObjectHolder annotations
        [01:09:06] [Server thread/INFO] [FML]: Identifying ItemStackHolder annotations
        [01:09:06] [Server thread/INFO] [FML]: Found 0 ItemStackHolder annotations
        [01:09:06] [Server thread/INFO] [FML]: Configured a dormant chunk cache size of 0
        [01:09:06] [Server thread/INFO] [FML]: Applying holder lookups
        [01:09:06] [Server thread/INFO] [FML]: Holder lookups applied
        [01:09:06] [Forge Version Check/INFO] [ForgeVersionCheck]: [Forge] Starting version check at
        http://files.minecraftforge.net/maven/net/minecraftforge/forge/promotions_slim.json
        [01:09:06] [Server thread/INFO] [FML]: Injecting itemstacks
        [01:09:06] [Server thread/INFO] [FML]: Itemstack injection complete
        [01:09:06] [Server thread/INFO]: Loading properties
        [01:09:06] [Server thread/INFO]: Default game type: SURVIVAL
        [01:09:06] [Server thread/INFO]: Generating keypair
        [01:09:06] [Forge Version Check/INFO] [ForgeVersionCheck]: [Forge] Found status: BETA_OUTDATED Target: 11.15.0.1684
        [01:09:07] [Server thread/INFO]: Starting Minecraft server on *:25565
        [01:09:07] [Server thread/INFO]: Using epoll channel type
        [01:09:07] [Server thread/INFO] [FML]: Injecting itemstacks
        [01:09:07] [Server thread/INFO] [FML]: Itemstack injection complete
        [01:09:07] [Server thread/INFO] [FML]: Forge Mod Loader has successfully loaded 3 mods
        [01:09:07] [Server thread/WARN]: Failed to load user banlist:
        java.io.FileNotFoundException: banned-players.json (No such file or directory)
        at java.io.FileInputStream.open(Native Method) ~[?:1.7.0_91]
        at java.io.FileInputStream.<init>(FileInputStream.java:146) ~[?:1.7.0_91]
        at com.google.common.io.Files.newReader(Files.java:86) ~[minecraft_server.1.8.9.jar:?]
        at net.minecraft.server.management.UserList.func_152679_g(SourceFile:124) ~[mb.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.func_152620_y(SourceFile:99) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.<init>(SourceFile:25) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedServer.func_71197_b(DedicatedServer.java:206) [ko.class:?]
        at net.minecraft.server.MinecraftServer.run(MinecraftServer.java:441) [MinecraftServer.class:?]
        at java.lang.Thread.run(Thread.java:745) [?:1.7.0_91]
        [01:09:07] [Server thread/WARN]: Failed to load ip banlist:
        java.io.FileNotFoundException: banned-ips.json (No such file or directory)
        at java.io.FileInputStream.open(Native Method) ~[?:1.7.0_91]
        at java.io.FileInputStream.<init>(FileInputStream.java:146) ~[?:1.7.0_91]
        at com.google.common.io.Files.newReader(Files.java:86) ~[minecraft_server.1.8.9.jar:?]
        at net.minecraft.server.management.UserList.func_152679_g(SourceFile:124) ~[mb.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.func_152619_x(SourceFile:91) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.<init>(SourceFile:27) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedServer.func_71197_b(DedicatedServer.java:206) [ko.class:?]
        at net.minecraft.server.MinecraftServer.run(MinecraftServer.java:441) [MinecraftServer.class:?]
        at java.lang.Thread.run(Thread.java:745) [?:1.7.0_91]
        [01:09:07] [Server thread/WARN]: Failed to load operators list:
        java.io.FileNotFoundException: ops.json (No such file or directory)
        at java.io.FileInputStream.open(Native Method) ~[?:1.7.0_91]
        at java.io.FileInputStream.<init>(FileInputStream.java:146) ~[?:1.7.0_91]
        at com.google.common.io.Files.newReader(Files.java:86) ~[minecraft_server.1.8.9.jar:?]
        at net.minecraft.server.management.UserList.func_152679_g(SourceFile:124) ~[mb.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.func_72417_t(SourceFile:107) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.<init>(SourceFile:29) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedServer.func_71197_b(DedicatedServer.java:206) [ko.class:?]
        at net.minecraft.server.MinecraftServer.run(MinecraftServer.java:441) [MinecraftServer.class:?]
        at java.lang.Thread.run(Thread.java:745) [?:1.7.0_91]
        [01:09:07] [Server thread/WARN]: Failed to load white-list:
        java.io.FileNotFoundException: whitelist.json (No such file or directory)
        at java.io.FileInputStream.open(Native Method) ~[?:1.7.0_91]
        at java.io.FileInputStream.<init>(FileInputStream.java:146) ~[?:1.7.0_91]
        at com.google.common.io.Files.newReader(Files.java:86) ~[minecraft_server.1.8.9.jar:?]
        at net.minecraft.server.management.UserList.func_152679_g(SourceFile:124) ~[mb.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.func_72418_v(SourceFile:123) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedPlayerList.<init>(SourceFile:30) [kn.class:?]
        at net.minecraft.server.dedicated.DedicatedServer.func_71197_b(DedicatedServer.java:206) [ko.class:?]
        at net.minecraft.server.MinecraftServer.run(MinecraftServer.java:441) [MinecraftServer.class:?]
        at java.lang.Thread.run(Thread.java:745) [?:1.7.0_91]
        [01:09:07] [Server thread/INFO]: Preparing level "world"
        [01:09:07] [Server thread/INFO] [FML]: Loading dimension 0 (world) (net.minecraft.server.dedicated.DedicatedServer@60d0e35f)
        [01:09:08] [Server thread/INFO] [FML]: Loading dimension 1 (world) (net.minecraft.server.dedicated.DedicatedServer@60d0e35f)
        [01:09:08] [Server thread/INFO] [FML]: Loading dimension -1 (world) (net.minecraft.server.dedicated.DedicatedServer@60d0e35f)
        [01:09:08] [Server thread/INFO]: Preparing start region for level 0
        [01:09:09] [Server thread/INFO]: Preparing spawn area: 3%
        [01:09:10] [Server thread/INFO]: Preparing spawn area: 5%
        [01:09:11] [Server thread/INFO]: Preparing spawn area: 8%
        [01:09:12] [Server thread/INFO]: Preparing spawn area: 11%
        [01:09:14] [Server thread/INFO]: Preparing spawn area: 20%
        [01:09:15] [Server thread/INFO]: Preparing spawn area: 28%
        [01:09:16] [Server thread/INFO]: Preparing spawn area: 31%
        [01:09:17] [Server thread/INFO]: Preparing spawn area: 36%
        [01:09:18] [Server thread/INFO]: Preparing spawn area: 39%
        [01:09:19] [Server thread/INFO]: Preparing spawn area: 49%
        [01:09:20] [Server thread/INFO]: Preparing spawn area: 58%
        [01:09:21] [Server thread/INFO]: Preparing spawn area: 72%
        [01:09:22] [Server thread/INFO]: Preparing spawn area: 86%
        [01:09:23] [Server thread/INFO]: Preparing spawn area: 94%
        [01:09:23] [Server thread/INFO]: Done (16.006s)! For help, type "help" or "?"
        >

    {: .note }
    > Don't worry about the errors displayed. They happen because the **banned-players.json**, **banned-ips.json**, **ops.json** and
    **whitelist.json** files are missing the first time the server starts; next time you start it, they will not appear anymore.
