# marvin README, updated for 1.0, 20150721

--

SUMMARY

marvin is a simple Don't Starve Together dedicated server management script.

marvin is written in bash (v4) for command-line use. It enables the user to easily install, start, stop, update, and otherwise manage a Don't Starve Together dedicated server installation.

This README file was written to provide an overview of what marvin can do, how it works, and how to use it.

--

FEATURES AND THEORY OF OPERATION

Executing marvin with the "help" argument will display the following helpful info:

marvin is a very simple Don't Starve Together dedicated server manager.

For more information, go here:
  http://forums.kleientertainment.com/topic/56384-marvin-a-simple-linux-dst-server-manager/

  marvin start
  marvin stop
  marvin restart
  marvin status
  marvin update
  marvin update-validate
  marvin console
  marvin setup



marvin uses tmux as a terminal emulator, instead of screen. If you want to use the console, you will need to be familar with some of the tmux meta commands. Note that tmux uses ctrl+b as it's meta command, whereas screen uses ctrl+a.

Below is a brief cheat-sheet of tmux commands and key combinations:

	ctrl+b d		Disconnect from the console session. Leave the server running.
	ctrl+b x		Kill the console session and the server ungracefully (not recomended).
	ctrl+b pgup/pgdn	Page up and down the tmux session history.

	# Dump the current console history to file:
	tmux capture-pane -S - -J -t DST-server:0.0 \; save-buffer console.log



By default, marvin installs the server and configuration/save-data in "~/dontstarvetogether".

IMPORTANT: marvin tells the DST server to use the "~/dontstarvetogether/config" directory for configuration rather than the default of "~/.klei". If you want to use the default instead, change CONFIG_DNAME and CONFIG_DIR to "CONFIG_DNAME=.klei" and "CONFIG_DIR=~/$CONFIG_DNAME".

--

LIMITATIONS

marvin should be pretty easy to use, but it's not for dummies. You need to know a little unix 101. Please don't ask me to help you teh unix or configure your firewall.

marvin is a command-line only tool. There is no pointy-clicky web interface. If you are not comfortable working on a command line over ssh, then marvin isn't for you.

marvin is written in bash v4-type script and uses a number of bash-isms and Linux-specific commands. marvin probably won't work on BSD and OSX without localized fixes.

marvin was written to manage a single installation. It can't do multiple server installations.

marvin has no support for helping you install or manage mods.

At this time, marvin can't automatically update the server installation. When a new server release is published, you must manually shut down your server, update it, and start it up again.

marvin isn't very smart. It doesn't have server crash detection, auto-restart/recovery capabilities, notification options, or much else.

--

CONFIGURATION PARAMETERS 

Almost all of marvin's configuration parameters are near the top of the script, but you shouldn't need to touch them. It should be fairly evident as to what each does.

The one parameter you may wish to change is INSTALL_DIR, which controls where marvin installs the server files. By default, this is "~/dontstarvetogether"

--

INSTALLATION

In order to use marvin, you must perform a few initial setup steps. This includes installing dependency applications, dependency libraries, configuring the default variables, modifying the host system where necessary, and making your settings.ini file.

It is recommended that marvin be placed in your user's ~/bin directory.

Running the "marvin setup" command will walk you through most of these steps.

marvin comes with a bash autocompletion script, "marvin.bash-autocompletion". This script will help your bash shell auto-complete commands by using the tab key. Source this file in some way from your profile. Some distributions will also automatically load this if you place it into your "~/.bash_completion" file. If that' not the case for you, add "source ~/.bash_completion" to your login script/.profile. This step is not required, but highly recommended.

marvin has a built-in dependency checker system as part of it's "setup" command. Running this command will show the user which tools are missing from the system. Most of these tools can be installed via your local GNU/Linux installation package management tool set, such as apt/dpkg, yum, pkg, etc.



On a Debian GNU/Linux host, the following command should take care of installing most dependencies:

# apt-get install lib32gcc1 tmux wget lsof sed coreutils grep findutils bsdutils util-linux procps



Obviously, if you are not a bash shell user, you will need to install a modern (v4) version of the bash shell as well.

marvin can optionally use nice/renice to raise the priority on running srcds installations if the user has permissions via limits.conf. It would be a really good idea to configure /etc/security/limits.conf to allow the marvin user to renice processes with a higher priority (10 is good). This is not a requirement, however.

If you want to start your server at system boot, use cron's "@reboot" command like so:

	@reboot        ~/bin/marvin bootstart

IMPORTANT: Be sure you have a sane PATH setting. Also make sure your cron tab has a sane path, as marvin doesn't set it's own path.

For example, put this at the top of your cron tab:

	SHELL=/bin/bash
	PATH=$HOME/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin

Be sure to "stop" of your server before rebooting or shutting down your host system.

Remember to configure your firewall as appropriate.

--

SAMPLE SETTINGS.INI FILE

DST doesn't come with a sample settings.ini file, and it won't run without one.

Below is a sample settings.ini file config:

	[network]
	default_server_name = My DST server name
	default_server_description = My DST server description
	server_password = potato
	; server_port = 10999
	; steam_master_server_port = 27016
	; steam_authentication_port = 8766
	max_players = 64
	pvp = false
	enable_vote_kick = true
	game_mode = endless
	enable_autosaver = true
	pause_when_empty = true

	[account]
	dedicated_lan_server = false
	server_token = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

	[STEAM]
	DISABLECLOUD = true

	[MISC]
	CONSOLE_ENABLED = true



For more information about the settings.ini file, please read the DST wiki:
	http://dont-starve-game.wikia.com/wiki/Guides/Don’t_Starve_Together_Dedicated_Servers

--

MISCELLANEOUS NOTES

marvin is a simplified/stripped-down fork of another game server manager named "wrench", which manages Valve's SRCDS game servers (TF2, CSGO, etc). As such, it has some artifacts which may not be used or might be confusing.

tmux previously had a serious redirection bug which can cause marvin to break badly. As a result, tmux version of 1.6 or greater is required. This should not be a problem for a modern distribution.

marvin probably won't work on Mac OSX because of the old 3.x version of BASH OSX ships with. I'm not sure SteamCMD or the DST server will run on a Mac anyway.

--

LICENCE

marvin and marvin_README.txt are herein assigned to the public domain.

