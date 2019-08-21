# Hardware

*  Boot Keys

| Key        | What it does	|
| ---------- |:-------------|
| Option		| Display all bootable volumes (Startup Manager)|
| Shift		| Perform Safe Boot(start up in Safe Mode)|
| C				| Start from a bootable disc|
| T				| Start in FireWire target disk mode|
| N				| Start from NetBoot server|
| X				| Force Mac OS X startup (if non-Mac OS X startup volumes are present)|
| Command-V	| Start in Verbose Mode|
| Command-S	| Start in Single User Mode|
| D				| Diagnostic Mode|
| Alt			| Bootcamp |

http://support.apple.com/kb/HT1343

* Resetting PRAM and NVRAM

1. Shut down the computer.
2. Locate the following keys on the keyboard: Command, Option, P, and R. You will need to hold these keys down simultaneously in step 4.
3. Turn on the computer.
4. Press and hold the Command-Option-P-R keys. You must press this key combination before the gray screen appears.

* Reset SMU

1. On the built-in keyboard, press the (left side) Shift-Control-Option keys and the power button at the same time.
2. Release all the keys and the power button at the same time.
3. Press the power button to turn on the compute


* Remap § with ~ on International Keyboard

	```
	hidutil property --set '{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000064,"HIDKeyboardModifierMappingDst":0x700000035},{"HIDKeyboardModifierMappingSrc":0x700000035,"HIDKeyboardModifierMappingDst":0x700000064}]}'

	```

# Filesystem

* Mac OS X Hidden Files & Directories

| File/Directory| Description   |
| ------------- |:-------------|
|._whatever	| These files are created on volumes that don't natively support full HFS file characteristics (e.g. ufs volumes, Windows fileshares, etc). When a Mac file is copied to such a volume, its data fork is stored under the file's regular name, and the additional HFS information (resource fork, type & creator codes, etc) is stored in a second file (in AppleDouble format), with a name that starts with "._". (These files are, of course, invisible as far as OS-X is concerned, but not to other OS's; this can sometimes be annoying...) |
|.DS_Store	| This file in created by the Finder to keep track of folder view options, icon positions, and other visual information about folders. A separate .DS_Store file is created in each directory to store information about that directory, so you'll find them appearing all over the directory tree, in pretty much every folder you've visited with the OS X Finder.|
|~/.Trash		| Used to store files & folders from the boot volume that a particular user has thrown in the trash, but that haven't been erased yet.|
|/.Spotlight-V100	| Used to store metadata indexes and indexing rules for Spotlight (version 1.00 apparently). Only created under Mac OS X 10.4.|
|/Volumes/(whatever)/.Trashes		| On volumes other than the boot volume, a .Trashes folder is used to hold files & folders that've been put in the trash but not yet deleted. Since each user has their own personal trash can, subfolders are created under .Trashes for different users, named according to their user ID number. For example, if user #501 throws something on a volume named "Data" into the trash, it'd be moved to a directory named /Volumes/Data/.Trashes/501/. Permissions on this folder are set so that you can only access a trash can if you can guess the users' ID -- that is, you cannot view a list of which users actually have trash cans in existance. If you're trying to free disk space, this can make it rather tricky to find & delete the files in other users' trash cans... |
|/.hidden |	This contains a list of files for the Finder to hide -- it's one of three ways a file can be made invisible in OS X. This file is semi-obsolete -- i.e. it does not exist in a standard installation of Mac OS X 10.4, but the Finder will still respect it if it exists...|
|/.hotfiles.btree	| Used to track commonly-used small files so their position on disk can be optimized (a process called "adaptive hot file clustering"). |
|/.vol		| This pseudo-directory is used to access files by their ID number (aka inode number) rather than by name. For example, /.vol/234881034/105486 is file #105486 on volume #234881034.|
|/automount		|	Used to handle "quasi-static" mounts of network volumes under OS X 10.1. Under most unixes, if a network volume is statically mounted on a client, it's mounted somewhere in the file system, so it looks shows up like a normal directory. Under OS X 10.1, a statically-mounted network volume will actually be mounted in /automount, and a symbolic link pointing to it will be placed where the volume would normally be mounted, thus emulating the normal result. (Compare this with how "network" mounts are handled via /private/Network.) |
|/bin		|	This is one of several places where unix-style binaries (that is, programs, or command-line commands) are kept. The programs in /bin include the more common and fundamental things that are used from the unix command line (e.g. ls and rm), as well as several shells (the programs that provide the command line itself). The other places where similar files are stored are /sbin, /usr/bin, /usr/sbin, and possibly /usr/local/bin, /usr/local/sbin, and maybe even ~/bin/powerpc-apple-macos; collectively, these can be thought of as the command line's equivalent of /Applications. |
|/cores	|	(This is actually stored in /private/cores; /cores is really just a symbolic link.) Under some conditions, when a program crashes, it'll "dump core" (essentially, store a copy of the program state at the time it crashed) into this directory. This is really only useful for programmers trying to debug their own programs. |
|/dev		| This directory contains what're technically known as device special files. These are not really files in the usual sense, they're more like placeholders that the system uses to keep track of the devices (disks, keyboards, monitors, network connections, etc) attached to it. |
|/etc		|	(This is actually stored in /private/etc; /etc is really just a symbolic link.) On a typical unix system the /etc folder will contain all the configuration files for a system, including both documents specifying config information as well as scripts for actually performing various configuration tasks. On OS X, some of the config information stored here is overridden by NetInfo or other directory services, but the /etc files still exist. |
|/lost+found		|If Disk Utility or fsck discover "orphaned" files (i.e. files that exist, but aren't actually in any directory), they'll be placed here. |
|/Network		|	This is the "real" location of the Network item that appears at the Computer level in the finder. It provides a place to attach network-wide resources and server volumes. Under OS X 10.1, network resources actually tend to get mounted in/private/Network, and symbolic links to them created in /Network. In OS 10.3, various network resources (mainly servers) appear dynamically in /Network (thanks to some virtual filesystem magic).|
|/mach /mach.sym /mach_kernel	|	The Mach kernel (which runs at the very core of Mac OS X), along with a couple of shortcuts for getting at it in various ways.
|/private	|	In OS X certain root level directories are actually symbolic links (similar to aliases) to directories in /private. Examples are /cores, /etc, and /var which are respectively linked to /private/cores, /private/etc, and /private/var. /private also contains a directory of drivers for certain peripherals. |
|/private/Network	| Used to handle "network" (non-static) mounts of network volumes under OS X 10.1. Under previous versions, network mounts were mounted in /Network, but in 10.1 they're actually mounted in /private/network, and a symbolic link is placed in /Networkpointing to the actual mount point. (Compare this with how "quasi-static" mounts are handled via /automount.)|
|/sbin	|	The /sbin directory is like /bin except it contains binaries that are specifically used for system administration (e.g. mount andfsck).|
|/tmp	|	(This is actually stored in /private/tmp; /tmp is really just a symbolic link.) Programs that need temporary space on the hard disk are usally set up to write temporary files to the /tmp directory (although some use /var/tmp instead).|
|/usr	| The /usr directory contains many subdirectories that have binaries and files specifically of use to the normal (unix) user. |
|/usr/bin	|	Another place where unix binaries are kept.|
|/usr/lib	| Libraries available for use by progrmming on Mac OS X. Unless you install the Developer Tools, this'll be mostly empty. Note that this has no relation to Mac OS X's various "Library" directories." |
|/usr/libexec	 | Holds various daemon programs, system maintenance scripts, and other unix-style programs that usually aren't run directly by humans.|
|/usr/local	|	As in most unixes, this directory is used to store local customizations and additions to the standard OS installation (e.g. /usr/local/bin would be likely to hold unix binaries added by the system administrator). This directory can be thought of roughly as the unix equivalent of Mac OS X's local library. In the standard install of Mac OS X it is (not surprisingly) completely empty. Note: As of OS X 10.2, these directories are no longer in the default search path for command-line executables; as a result, anything installed here will not be useable without taking extra steps of one sort or another. |
|/usr/sbin	| Yet another place where unix binaries are kept.|
|/usr/share	| Contains various data and text files that can, in principle, be shared by multiple architectures (a distinction which makes a lot more sense under other flavors of unix than it does under Mac OS X). |
|/usr/standalone		| Contains boot loader programs for (potentially) various computer architectures. In the installs I've looked at, this is simply a duplicate of the BootX loader (also found in /System/Library/CoreServices/BootX); I'm not sure why both copies are needed. |
|/var	|	(This is actually stored in /private/var; /var is really just a symbolic link.) Sometimes processes controlled by the operating system need a place to store variable files. Processes like printing and programs that store log files will use subdirectories in the /var directory to store those files. It also holds a fair bit of configuration information (especially in /var/db). |
|/var/backups		| Used to store backups of critical system information (mainly, the nightly dumps of NetInfo databases). |
|/var/db	| Holds various databases of system information. The most notable are the NetInfo databases (stored in /var/db/netinfo), shadow password files (in /var/db/shadow/hash), and the system's network configuration database (/var/db/SystemConfiguration/preferences.xml - although it moved to /Library/Preferences/SystemConfiguration/ in 10.3), which together store much of the system and network configuration information that a traditional unix admin would expect to find in /etc, and a Mac OS 9 admin would expect to find in System Folder:Preferences. |
|/var/log	| This is where many of the system event logs are kept (others are kept in /Library/Logs). |
|/var/root	| The root (superuser) account's home directory. Note that this directory will exist even if you haven't enabled the root account. |
|/var/run	 | Stores various status information about processes (especially daemons) running on the system. |
|/var/tmp	 | A place for programs to store temporary data, just like /tmp. Some programs use one, some use the other, so Mac OS X provides both. |
|/var/vm	 | Used to store the swap files for Mac OS X's virtual memory. |
|/var/vm/app_profile	| Holds information about various applications' virtual memory useage.|
|/Volumes	| The /Volumes directory is the mount point for all of the drives (other than the boot volume) connected to the system. The Finder hides the Volumes directory itself, but displays its contents at the Computer level. |


* Hide Folder

	`chflags hidden /path/to/file/or/folder/`

	`chflags nohidden /path/to/unhide/`

* Zip folder with password

	`zip -e protected.zip /file/to/protect/`


* Remove Dot Files

	`dot_clean . (removes dot files)`

* shows all files in finder

	`defaults write com.apple.Finder AppleShowAllFiles 1`

* Search using spotlight
	`mdfind "IBM documents”`

* Search using spotlight only one directory

	`mdfind -onlyin ~/Documents essay`

* Erase and rebuild spotlight index

	`mdutil -E`

* Turn off indexing

	`dutil -i off`

* Verify Disk Permissions

	`sudo /usr/libexec/repair_packages --verify --standard-pkgs /`

* Repair Disk permissions

	`sudo /usr/libexec/repair_packages --repair --standard-pkgs --volume /  `


* Copy the path of the file:

 	`Press the Option key in a file context menu `


* verify that the SHA-1 digest of any download matches the digest published there

	`/usr/bin/openssl sha1 download.dmg`

* Remove DS Store Files

	`sudo find / -name ".DS_Store" -depth -exec rm {} \;`

# Other



* Find All Startup items

	`sudo launchctl list`

* Have script start apache on startup

	`sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist`

* Unload a script on startup

	`sudo launchctl unload [path/to/script] -w (permanently removes it)`

* Launchd scripts are stored in the folllowing locations:

	```
	~/Library/LaunchAgents
	/Library/LaunchAgents
	/Library/LaunchDaemons
	/System/Library/LaunchAgents
	/System/Library/LaunchDaemons
	```


* To see the path names of disks

	`diskutil list`

* Find all wifinetworks

	```
	defaults read /Library/Preferences/SystemConfiguration/com.apple.airport.preferences RememberedNetworks | 	egrep -o '(SSID_STR|_timeStamp).+' | sed 's/^.*= \(.*\);$/\1/' | sed 's/^"\(.*\)"$/\1/' | sed 's/\	([0-9]\{4\}-..-..\).*/\1/'
	```


* Opensnoop

	`sudo opensnoop`

* Speak Something

	`say "Hello there."`


* Crop Image:

	`sips -Z 100x100 image.jpg`


* Prevent Screensaver
	`caffeinate -t 3600 `

* open multiple instances
	`open -n /Applications/Safari.app/`

* Show everything you've ever downloaded

	`sqlite3 ~/Library/Preferences/com.apple.LaunchServices.QuarantineEventsV* 'select LSQuarantineDataURLString from LSQuarantineEvent' |more`

* To Delete your download history

	`sqlite3 ~/Library/Preferences/com.apple.LaunchServices.QuarantineEventsV* 'delete from LSQuarantineEvent'`

* Track Process

	`sudo opensnoop -n Safari`

* You can also track a specific file, and what is accessing it, like so:

	`sudo opensnoop -f /etc/hosts`

* Tracking a specific process is as simple as just specifying the process id:

	`sudo opensnoop -p PID`


* show which processes are using network

	`nettop -`

* shows open file system files

	`fs_usage`

* increase or decrease your volume by quarter increments:

	`option+shift + volume`


* restart the Mac:

	`ctrl + ⌘ + ⏏`

* kill not responding programs (including the Finder)

  	`option + ⌘ + esc  Select Force Quit from the menu that appears.`


* Quitting Apps

	`While you are using ⌘ + ⇥ to cycle through open applications, you can press Q before you release ⌘ to close the app`

* Terminal


* Finder

	* ⌘+Shift+A/U/D: These three-in-one shortcuts take you to the Applications, Utilities, and Desktop folders (respectively) when in the Finder. Because you'll need to get to each relatively often, this key command can save you quite a bit of time.

	* ⌘+1/2/3/4: When you need to change views in the finder, you don't have to bother with your mouse. 1 will get you icon view, 2 list view, 3 column view, and 4 cover flow.

	* ⌘+Option+I: When you need info on multiple files, just select them and execute this key command. You'll get an info panel about everything currently selected.

	* ⌘+Shift+4 and Space: When you press Command+Shift+4 you get to take a screenshot of a specific area on the screen. If you hit the space bar afterwards, however, you can click on any window to get a nice PNG with transparent background of that window.


	* ⌘+Option+Space: Use the option key to get a Spotlight search window and get more specific about what you're trying to find.


* Searching for commands shortcuts

	`⌘?`

* Open A file in finder from Spotlight

	`Hold down ⌘ when clicking it`

* Show other open windows

	`⌘+Tab + Hold ⌘ + Up arrow`


* Change the screencapure to different format

	`defaults write com.apple.screencapture type PDF`


* Change Auto Update Frequency to 3 days

	`sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate ScheduleFrequency 3`

* Paste Text without formatting

	`Command++Shift+V`

* Find last hibernate/sleep time

	`pmset -g log | grep sleep | tail -n 2`

* Query Bonjour

	`dscacheutil -q host -a name My-MacBook-Pro.local`

* Change default screenshot to jpg

	`sudo defaults write com.apple.screencapture type jpg`

	`sudo killall SystemUIServer`

* Textedit prevent new document popup

	`defaults write -g NSShowAppCentricOpenPanelInsteadOfUntitledFile -bool false`




* To toggle hidden files In Finder:

 	`Shift+Cmd+.`

* select the output of the previous command.:

 	`cmd-shift-A.`

* This scrolls you back up to the previous command.

  	`Cmd-Up`

* Show local time machine snapshots

	`tmutil listlocalsnapshotdates`

* Remove local snapshots

	`tmutil deletelocalsnapshots`

* Show Hidden files

	`defaults write com.apple.finder AppleShowAllFiles true; killall Finder`

* Show Hidden files in Finder

	`Command-Shift-.`

# Security

*  Disable guest account and sharing:

	`Select the Guest Account and then disable it by unchecking "Allow Guest to log in to this computer." Uncheck "Allow guests to connect to shared folders."`

* More Security

	* Require password "5 seconds" after sleep or screen saver begins
	* Disable automatic login
	* Use secure virtual memory
	* Disable Location Services (if present)
	* Disable remote control infrared receiver (if present)
	* FileVault is recommended for portable systems since it can protect data even if the system is stolen.
	* In the Firewall tab, click "Start" to turn firewall on. Next, click on "Advanced..." and enable "Block all incoming connections."

* Secure Users' Home Folder Permissions

	To prevent users and guests from perusing other users' home folders, run the following command for each home folder:

	`sudo chmod go-rx /Users/username`

* Disable Unnecessary Services.

The following services can be found in /System/Library/ LaunchDaemons. Unless needed for the purpose shown in the second column, disable each service using the command below, which needs the full path specified:
  sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.blued.plist

| Filename   | Needed for|
| ---------- |:----------|
|com.apple.blued.plist	|	Bluetooth|
|com.apple.IIDCAssistant.plist | iSight |
|com.apple.nis.ypbind.plist | NIS |
|com.apple.racoon.plist	| VPN |
|com.apple.RemoteDesktop.PrivilegeProxy.plist| ARD|
|com.apple.RFBEventHelper.plist |ARD
|com.apple.UserNotificationCenter.plist| User notifications
|com.apple.webdavfs_load_kext.plist | WebDAV
|org.postfix.master | email server



The following services can be found in /System/Library/ LaunchAgents. Disable them in the same way.

| Filename   | Needed for|
| ---------- |:----------|
| com.apple.RemoteUI.plist |Remote Control|
| com.apple.RemoteDesktop.plist| ARD|

* Au Revoir, Bonjour!

Bonjour is Apple's implementation of Zeroconf which provides a network service discovery protocol. Using Bonjour, many programs advertise their services on the local network to facilitate configuration. While this may be beneficial in some cases, from the security perspective this makes the computer unnecessarily visible and generates unwanted network traffic.
Disable Bonjour's multicast advertisements with the following command and reboot:

	```
	sudo defaults write /System/Library/
	LaunchDaemons/com.apple.mDNSResponder
	ProgramArguments -array-add  "-NoMulticastAdvertisements"
	```

# Other Links


https://github.com/herrbischoff/awesome-osx-command-line - mac shortcuts


# Chrome

Shortcuts

	Command + t reopen closed tab
	Command + y = history
	Command + 1  = first tab
	Command + r = reload
	Command + Shift + n = incognito
	Command + t - new tab
	Command + w - close tab
	Command + Shift + T - reopen last tab
	Command + option + > - cycle through tabs
	Command + number = goes to tab number
	Command + n = new window
	Command + Shift + ~ = cycle between windows
	Command + l = go to search
	Command + r = reload
	Command + Shift + r = hard reload
	Control + tab - cycle through tabs

# Github

	t - search for a file

# Gmail

	jk - move up and down
	Shift + # - delete email

# Finder

	Cmd ⌘ ` - Cycle through open windows
	Cmd ⌘ Shift ⇧ ` - Cycle through open windows in reverse
	Command + Tab until you get the app's icon. Before releasing the Command key, press and hold the Option key - Restore minimized window

# Window Management

	Command-~ to switch between windows of the current program

	
* hide window

	`command + h`

	* ⌘+Option+M:  Minimize all windows to the dock with this keyboard shortcut.

	* ⌘+(Shift)+~: If you'd rather cycle through millions of windows, you can use this key command to do so. Add or remove the shift key to change directions.

# Text Editing


	Command-e, put the selected text on the find clipboard.
	Command-g (find next) will search for the selected text without sacrificing the copy/paste clipboard
	
	cntrl+a: go to start of a line
	cntrl+p: go up one line
	
	cntrl+n: go down one line
	
	cntrl+k: cut line proceeding cursor
	
	cntrl+f: forward one char
	
	cntrl+b: back one char
	
	`ctrl+A: beginning of line`

	`ctrl+E: end of line`
	
	Shift-option-command-v: paste text without any formatting 
	
	* Insert an emoji anywhere:

 	`Ctrl+Cmd+Space.`
	
	-option left or right to move word by word
	-option click to move cursor in terminal to mouse
	-option, click+ select, option delete, deletes section
	-ctrl+D is del key
	-ctrl+K deletes from cursor to end of line

# Screenshots

	* Command-Shift-3: Take a screenshot of the screen, and save it as a file on the desktop

	* Command-Shift-4, then select an area: Take a screenshot of an area and save it as a file on the desktop

	* Command-Shift-4, then space, then click a window: Take a screenshot of a window and save it as a file on the desktop

	* Command-Control-Shift-3: Take a screenshot of the screen, and save it to the clipboard

	* Command-Control-Shift-4, then select an area: Take a screenshot of an area and save it to the clipboard

	* Command-Control-Shift-4, then space, then click a window: Take a screenshot of a window and save it to the clipboard

* Copy window screenshot to clipboard

	`screencapture -c -W`

* Capture entire screen to new email

	`screencapture -C -M image.png`

# Google Search

	Search
	[sunset]
	[watch]
	“SFO Terminal 1”
	Youtube
	use the numbered keys to seek in a video. For example, hitting “2” will take you 20 percent 
	File_name_V2: Freeze moments in time by naming different versions of the docs you edit frequently. In a Doc, Sheet, or Slides go to File > Version History > Name current version. Name any version then access it easily from "Version history" by name.
	
	
	OR
	Examples: jobs OR gates / jobs | gates
	
	AND
	jobs AND gates
	
	Grouping
	(ipad OR iphone) apple
	
	Cache
	cache:apple.com
	
	Filetype
	apple filetype:pdf / apple ext:pdf
	
	Number range
	Example: wwdc video 2010..2014
