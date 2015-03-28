# XGamer #

XGamer is a launcher for starting games or applications in a second X11 session. It can be launched from the command line or has a Gtk2 GUI for managing and launching games. It has integration with openbox (a light-weight window manager) to provide a suitable environment for running processor or graphics intensive applications.

By running in a second session games can run at the same time as compositing programs such as _Compiz_ in your main session. It It also isolates your primary X11 session from unstable games and applications preventing bugs or crashes propagating to other applications. Running a second session can also take advantage of multi-core processors.

## Installing ##

To install XGamer first extract the source tarball - since you are reading this you may already have done this step:

```
tar zxvf xgamer-*.tar.gz
```

Then run the build configuration script:

```
perl Build.PL
```

Or to specify the prefix to install to:

```
perl Build.PL --prefix /usr
```

This will let you know if you are missing any dependencies so you can download and install them; they will probably be available through your distribution package manager. Then build and install with:

```
./Build
sudo ./Build install
```

Once it has installed you can run it with:

```
xgamer
```

### Permissions ###

If you are running Ubuntu or experience an error 'X: user not authorized to run the X server, aborting.' you need to allow user level access to X11 sessions. On _Debian_ or _Ubuntu_ you can run:

```
sudo dpkg-reconfigure x11-common
```

And change the permission to 'anybody'. On other systems you can modify the config file directly '/etc/X11/Xwrapper.config' to set 'allowed\_users=anybody'.

### Changes ###

There have been a number of changes in XGamer 0.6.1 that have broken backwards compatibility with previous versions. These are the fixes if you are upgrading from a previous version.

Version 0.6.1 has changed the formatting of its plain text configuration file. Entries for games like "Game=" has a list of game properties separated by commas ':' rather than by tabs '\t' this is to make them more human readable.

To update a configuration file from before 0.6.1 you will need to run:

```
sed -i "s/\t/:/g" $HOME/.xgamer/xgamer
```

There have been a couple of files removed version 0.6.1 that are no longer necessary:

```
${PREFIX}/share/xgamer/xgamer(2).glade
${PREFIX}/share/xgamer/xgamer.xml
```

Where ${PREFIX} is the installation root for XGamer, this may be something such as "/usr/local" or "/usr" and will contain "bin/xgamer" and "share/xgamer" among others.

These 2 files can now safely be removed.

Since version 0.1.1 xgamer has changed the location of the local configuration file from:

```
$HOME/.xgamer
```

To:

```
$HOME/.xgamer/xgamer
```

Version 0.6.1 has removed the check for this file so if you happen to be upgrading all the way from version 0.1.1 to 0.6.1 you will need to manually setup the files correctly like so:

```
mv $HOME/.xgamer $HOME/.xgamer.config
mkdir $HOME/.xgamer
mv $HOME/.xgamer.config $HOME/.xgamer/xgamer
```

## Using ##

You can use XGamer to launch games from the GUI, but if you are lazy and don't want to wait for those extra clicks you can create launchers for applications directly from the menu. Using your favourite menu editor (or by directly creating a .desktop file) you can add an menu entry with the command:

```
xgamer My Game Name
```

Where "My Game Name" is the name of the application - as seen in XGamer - to be launched.

It is also possible to supply the command directly with:

```
xgamer -c "/usr/bin/mygame"
```

## Configuring ##

XGamer can either be configured using the GUI (**Edit->Preferences**) or by manually editing the pain text configuration file ($HOME/.xgamer/xgamer). These are the available options:

```
Display / display to open new X11 server on, defaults to 1
Background / path to background wallpaper to display in XGamer session
Run 0..9 / commands to run on startup of XGamer session
```

These are set in the Xgamer configuration file like so:

```
Display=1
Background=/path/to/my/wallpaper.png
Run0=numlockx off
Run1=trayer
Game=Bounce:/usr/bin/bounce:/usr/share/pixmaps/bounce.svg:Bounce the ball!
Game=Pong:pong:x-pong:Bat the ball!
```

Here the last two configuration options are for games, they are colon ':' separated lists with 4 options. They read like so:

```
Game=GAME-NAME:EXECUTE:ICON:COMMENTS
```

Games can be added using the GUI using **File->Add**, also games can be imported from the menu by selecting their ".desktop" file **File->Import Game**.

There are a couple of recommended commands to set to run on XGamer startup:

```
numlockx on
trayer --transparent true --alpha 255 --widthtype request --edge top --align right
```

These turn on the numlock key by default and create a notification tray so applications that have notification icons (such as Steam) display properly.

## More ##

More documentation can be found by running:

```
man xgamer
```