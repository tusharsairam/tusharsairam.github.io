- [Download Syncthing](https://github.com/Bill-Stewart/SyncthingWindowsSetup/) from here. During download, I opted for creating a Windows Firewall rule for security purposes
- Syncthing should be installed in Android as well
- In the Syncthing configuration page, set the vault folder you want to sync under **Folders**
	- *Add Folder* -> Choose a *Folder Label* on your own as well as a *Folder ID*. The *Folder Path* should be the path to your vault

>[!warning] Folder IDs should match for syncing
>Make sure that the ==folder ID is the same== on all devices. Syncthing ties the IDs together and matches them to enable syncing

- Next, *Add Remote Device* in the **Remote Devices** section to add your phone
	- A *Device ID* will be asked for. Usually, the ID will already be detected and Syncthing will give you the option to use the ID of any nearby device that has Syncthing installed and running
	- Set your *Device Name*
	- In the *Sharing* tab, check the box corresponding to the Obsidian vault you want to share. Prior to checking, it will be under the *Unshared Folders* field
- Upon completing this, you will get a notification on your phone. Syncthiing needs your permission to accept the connection request from the laptop (originator). Accept it
- It's not over yet! You will see that the folder is still unshared and the sync status in **Remote Devices** shows *Syncing (0%)*. To finish the setup, go to the Syncthing app in your phone and create a new folder. ==Make sure you set the same folder ID here as you did earlier==. The folder name can be different