DOSBox v0.74-2 Manual (always use the latest version from www.dosbox.com)

# NOTE

While we are hoping that one day DOSBox will run all programs ever made for
the PC, we are not there yet.
At present, DOSBox running on a high-end machine will roughly be the equivalent
of a Pentium I PC. DOSBox can be configured to run a wide range of DOS games,
from CGA/Tandy/PCjr classics up to games from the Quake era.



# INDEX

1. Quickstart
2. Start (FAQ)
3. Command Line Parameters
4. Internal Programs
5. Special Keys
6. Joystick/Gamepad
7. KeyMapper
8. Keyboard Layout
9. Serial Multiplayer feature
10. How to speed up/slow down DOSBox
11. Troubleshooting
12. DOSBox Status Window
13. The configuration (options) file
14. The language file
15. Building your own version of DOSBox
16. Special thanks
17. Contact



# Quickstart

Type INTRO in DOSBox for a quick tour.
It is essential that you get familiar with the idea of mounting, DOSBox does not
automatically make any drive (or a part of it) accessible to the emulation. See
the FAQ entry "How to start?" as well as the description of the MOUNT command
(Section 4: "Internal Programs"). If you have your game on a cdrom you may try
this guide: https://www.vogons.org/viewtopic.php?t=8933



# Start (FAQ)

```
START:      How to start?
AUTOMATION: Do I always have to type these "mount" commands?
FULLSCREEN: How do I change to fullscreen?
FULLSCREEN: My fullscreen is too large.
CD-ROM:     My CD-ROM doesn't work.
CD-ROM:     The game/application can't find its CD-ROM.
MOUSE:      The mouse doesn't work.
SOUND:      There is no sound.
SOUND:      What sound hardware does DOSBox presently emulate?
SOUND:      The sound stutters or sounds stretched/weird.
KEYBOARD:   I can't type \ or : in DOSBox.
KEYBOARD:   Right Shift and "\" doesn't work in DOSBox. (Windows only)
KEYBOARD:   The keyboard lags.
CONTROL:    The character/cursor/mouse pointer always moves into one direction!
SPEED:      The game/application runs much too slow/too fast!
CRASH:      The game/application does not run at all/crashes!
CRASH:      DOSBox crashes on startup!
GAME:       My Build game(Duke3D/Blood/Shadow Warrior) has problems.
SAFETY:     Can DOSBox harm my computer?
OPTIONS:    I would like to change DOSBox's options.
HELP:       Great Manual, but I still don't get it.
```


## START: How to start?
    At the beginning you've got a Z:\> instead of a C:\> at the prompt.
    You have to make your directories available as drives in DOSBox by using
    the "mount" command. For example, in Windows "mount C D:\GAMES" will give
    you a C drive in DOSBox which points to your Windows D:\GAMES directory
    (that was created before). In Linux, "mount c /home/username" will give you
    a C drive in DOSBox which points to /home/username in Linux.
    To change to the drive mounted like above, type "C:". If everything went
    fine, DOSBox will display the prompt "C:\>".


## AUTOMATION: Do I always have to type these commands?
    In the DOSBox configuration file is an [autoexec] section. The commands
    present there are run when DOSBox starts, so you can use this section
    for the mounting. Look at Section 13: "The configuration (options) file".


## FULLSCREEN: How do I change to fullscreen?
    Press alt-enter. Alternatively: Edit the configuration file of DOSBox and
    change the option fullscreen=false to fullscreen=true. If fullscreen looks
    wrong in your opinion: Play with the options: fullresolution, output and
    aspect in the configuration file of DOSBox. To get back from fullscreen
    mode: Press alt-enter again.


## FULLSCREEN: My fullscreen is too large.
    This is can be a problem on Windows 10, if you have display scaling
    set to a value above 100%. Windows in that case will resize the screen
    on top of dosbox resizing the screen, which can happen for the output:
    ddraw, opengl, openglnb, overlay. You can disable this Windows behaviour
    by enabling a specific compatibility setting:
 
    - Right-click the DOSBox icon and select "Properties".
    - Go to the "Compatibility" tab.
    - Click on "Change high DPI settings".
    - Tick "Override high DPI scaling behaviour" and set it to "Application".
    - Apply the changes by clicking on "OK".

    Unfortunately, this compatibility option causes some side effects in
    windowed mode, and in this case you will need to change the resolution
    in the config/Options file for windowresolution (e.g. 1024x768).

    Alternatively, you can disable the display scaling and or use a lower
    fullresolution value.

## CD-ROM: My CD-ROM doesn't work.
    To mount your CD-ROM in DOSBox you have to specify some additional options
    when mounting the CD-ROM.
    To enable CD-ROM support (includes MSCDEX) in Windows:
      - mount d f:\ -t cdrom
    in Linux:
      - mount d /media/cdrom -t cdrom

    In some cases you might want to use a different CD-ROM interface,
    for example if CD audio does not work:
      To enable SDL-support (does not include low-level CD access!):
        - mount d f:\ -t cdrom -usecd 0 -noioctl
      To enable ioctl access using digital audio extraction for CD audio
      (Windows-only, useful for Vista):
        - mount d f:\ -t cdrom -ioctl_dx
      To enable ioctl access using MCI for CD audio (Windows-only):
        - mount d f:\ -t cdrom -ioctl_mci
      To force ioctl-only access (Windows-only):
        - mount d f:\ -t cdrom -ioctl_dio
      To enable low-level aspi-support (win98 with aspi-layer installed):
        - mount d f:\ -t cdrom -aspi

    explanation: - d   driveletter you will get in DOSBox (d is the best,
                          don't change it!)
                 - f:\ location of CD-ROM on your PC. In most cases it will
                          be d:\ or e:\
                 - 0   The number of the CD-ROM drive, reported by "mount -cd"
                          (note that this value is only needed when using SDL
                          for CD audio, otherwise it is ignored)
    See also the next question: The game/application can't find its CD-ROM.


## CD-ROM: The game/application can't find its CD-ROM.
    Be sure to mount the CD-ROM with -t cdrom switch, this will enable the
    MSCDEX interface required by DOS games to interface with CD-ROMs.
    Also try adding the correct label (-label LABEL) to the mount command,
    where LABEL is the CD-label (volume ID) of the CD-ROM.
    Under Windows you can specify -ioctl, -aspi or -noioctl. Look at the
    description of the mount command in Section 4: "Internal programs"
    for their meaning and the
    additional audio-CD related options -ioctl_dx, -ioctl_mci, -ioctl_dio.

    Try creating a CD-ROM image (preferably CUE/BIN pair) and use the
    DOSBox's internal IMGMOUNT tool to mount the image (the CUE sheet).
    This enables very good low-level CD-ROM support on any operating system.


## MOUSE: The mouse doesn't work.
    Usually, DOSBox detects when a game uses mouse control. When you click on
    the screen it should get locked (confined to the DOSBox window) and work.
    With certain games, the DOSBox mouse detection doesn't work. In that case
    you will have to lock the mouse manually by pressing CTRL-F10.


## SOUND: There is no sound.
    Be sure that the sound is correctly configured in the game. This might be
    done during the installation or with a setup/setsound utility that
    accompanies the game. First see if an autodetection option is provided. If
    there is none try selecting SoundBlaster or SoundBlaster 16 with the default
    settings being "address=220 irq=7 dma=1" (sometimes highdma=5). You might
    also want to select Sound Canvas/SCC/MPU-401/General MIDI/Wave Blaster
    at "address=330 IRQ=2" as music device.
    The parameters of the emulated sound cards can be changed in the DOSBox
    configuration file.
    If you still don't get any sound set the core to normal in DOSBox
    configuration and use some lower fixed cycles value (like cycles=2000). Also
    assure that your host operating sound does provide sound.
    In certain cases it might be useful to use a different emulated sound device
    like a SoundBlaster Pro (sbtype=sbpro1 in the DOSBox configuration file) or
    the Gravis Ultrasound (gus=true).


## SOUND: What sound hardware does DOSBox presently emulate?
    DOSBox emulates several legacy sound devices:
    - Internal PC speaker/Buzzer
      This emulation includes both the tone generator and several forms of
      digital sound output through the internal speaker.
    - Creative CMS/Gameblaster
      The is the first card released by Creative Labs(R).  The default
      configuration places it on address 220. It is disabled by default.
    - Tandy 3 voice
      The emulation of this sound hardware is complete with the exception of
      the noise channel. The noise channel is not very well documented and as
      such is only a best guess as to the sound's accuracy. It is disabled as
      default.
    - Tandy DAC
      Some games may require turning off SoundBlaster emulation (sbtype=none)
      for better Tandy DAC sound support. Don't forget to set the sbtype back to
      sb16 if you don't use Tandy sound.
    - Adlib
      This emulation is almost perfect and includes the Adlib's ability to
      almost play digitized sound. Placed at address 220 (also on 388).
    - SoundBlaster 16 / SoundBlaster Pro I & II / SoundBlaster I & II
      By default DOSBox provides SoundBlaster 16 level 16-bit stereo sound.
      You can select a different SoundBlaster version in the configuration of
      DOSBox. AWE32 music is not emulated as you can use MPU-401 instead
      (see below).
    - Disney Sound Source and Covox Speech Thing
      Using the printer port, this sound device outputs digital sound only.
      Placed at LPT1
    - Gravis Ultrasound
      The emulation of this hardware is nearly complete, though the MIDI
      capabilities have been left out, since an MPU-401 has been emulated
      in other code. For Gravis music you also have to install Gravis drivers
      inside DOSBox. It is disabled by default.
    - MPU-401
      A MIDI passthrough interface is also emulated. This method of sound
      output will only work when used with external device/emulator.
      Every Windows XP/Vista/7 and Mac OS X has got a default emulator 
      compatible with: Sound Canvas/SCC/General Standard/General MIDI/Wave 
      Blaster. A different device/emulator is needed for 
      Roland LAPC/CM-32L/MT-32 compatibility.


## SOUND: The sound stutters or sounds stretched/weird.
    You may be using too much CPU power to keep DOSBox running at the current
    speed. You can lower the cycles, skip frames, reduce the sampling rate of
    the respective sound device, increase the prebuffer. See Section 13: "The
    configuration (options) file".
    If you are using 'cycles=max' or 'cycles=auto', then make sure that there is
    no background processes interfering! (especially if they access the harddisk)
    Also look at Section 10: "How to speed up/slow down DOSBox" as well as
    Section 11: "Troubleshooting".


## KEYBOARD: I can't type \ or : in DOSBox.
    This can happen in various cases, like your host keyboard layout does not
    have a matching DOS layout representation (or it was not correctly
    detected), or the key mapping is wrong.
    Some possible fixes:
      1. Use / instead, or ALT-58 for : and ALT-92 for \.
      2. Change the DOS keyboard layout (see Section 8: "Keyboard Layout").
      3. Add the commands you want to execute to the [autoexec] section
         of the DOSBox configuration file.
      4. Open the DOSBox configuration file and change the usescancodes entry.
      5. Switch the keyboard layout of your operating system.

    Note that if the host layout can not be identified, or keyboardlayout is
    set to none in the DOSBox configuration file, the standard US layout is
    used. In this configuration try the keys around "enter" for the key \
    (backslash), and for the key : (colon) use shift and the keys between
    "enter" and "L".


## KEYBOARD: Right Shift and "\" doesn't work in DOSBox. (Windows only)
    This may happen if Windows thinks that you have more than one keyboard
    connected to your PC when you use some remote control devices.
    To verity this problem run cmd.exe, navigate to DOSBox program folder
    and type:
    set sdl_videodriver=windib
    dosbox.exe
    check whether keyboard started to work properly. As windib is slower it is
    best to use one of the two solutions provided here:
    https://www.vogons.org/viewtopic.php?t=24072


## KEYBOARD: The keyboard lags.
    Lower the priority setting in the DOSBox configuration file, for example
    set "priority=normal,normal". You might also want to try lowering the
    cycles (use a fixed cycle amount to start with, like cycles=10000).


## CONTROL: The character/cursor/mouse pointer always moves into one direction!
    See if it still happens if you disable the joystick emulation,
    set joysticktype=none in the [joystick] section of your DOSBox
    configuration file. Maybe also try unplugging any joystick/gamepad.
    If you want to use the joystick in the game, try setting timed=false
    and be sure to calibrate the joystick (both in your OS as well as
    in the game or the game's setup program).


## SPEED: The game/application runs much too slow/too fast!
    Look at Section 10: "How to speed up/slow down DOSBox" for more
    information.


## CRASH: The game/application does not run at all/crashes!
    Look at Section 11: "Troubleshooting".


## CRASH: DOSBox crashes on startup!
    Look at Section 11: "Troubleshooting".


## GAME: My Build game(Duke3D/Blood/Shadow Warrior) has problems.
    First of all, try to find a port of the game. Those will offer a better
    experience. To fix the graphics problem that occurs in DOSBox on higher
    resolutions: Open the configuration file of DOSBox and search for
    machine=svga_s3. Change svga_s3 to vesa_nolfb
    Change memsize=16 to memsize=63


## SAFETY: Can DOSBox harm my computer?
    DOSBox can not harm your computer more than any other resource demanding
    program. Increasing the cycles does not overclock your real CPU.
    Setting the cycles too high has a negative performance effect on the
    software running inside DOSBox.


## OPTIONS: I would like to change DOSBox's options.
    Look at Section 13: "The configuration (options) file".


## HELP: Great Manual, but I still don't get it.
    For more questions read the rest of this Manual. You may also look at:
    guides located at https://www.vogons.org/viewforum.php?f=53
    the wiki of DOSBox https://www.dosbox.com/wiki/
    the site/forum: https://www.dosbox.com



# Command Line Parameters

An overview of the command line options you can give to DOSBox. Although
in most cases it is easier to use DOSBox's configuration file instead.
See Section 13: "The configuration (options) file".

To be able to use Command Line Parameters:
(Windows)  open cmd.exe or command.com or edit the shortcut to dosbox.exe
(Linux)    use console
(Mac OS X) start terminal.app and navigate to:
           /applications/dosbox.app/contents/macos/dosbox

The options are valid for all operating systems unless noted in the option
description:

```
dosbox [name] [-exit] [-c command] [-fullscreen] [-userconf]
       [-conf congfigfilelocation] [-lang languagefilelocation]
       [-machine machine type] [-noconsole] [-startmapper] [-noautoexec]
       [-securemode] [-scaler scaler | -forcescaler scaler] [-version]
       [-socket socket]
       
dosbox -version
dosbox -editconf program
dosbox -opencaptures program
dosbox -printconf
dosbox -eraseconf
dosbox -erasemapper

  name
        If "name" is a directory it will mount that as the C: drive.
        If "name" is an executable it will mount the directory of "name"
        as the C: drive and execute "name".

  -exit
        DOSBox will close itself when the DOS application "name" ends.

  -c command
        Runs the specified command before running "name". Multiple commands
        can be specified. Each command should start with "-c" though.
        A command can be: an Internal Program, a DOS command or an executable
        on a mounted drive.

  -fullscreen
        Starts DOSBox in fullscreen mode.

  -userconf
        Start DOSBox with the users specific configuration file. Can be used
        together with multiple -conf parameters, but -userconf will always be 
        loaded before them.

  -conf configfilelocation
        Start DOSBox with the options specified in "configfilelocation".
        Multiple -conf options may be present.
        See Section 13: "The configuration (options) file" for more details.

  -lang languagefilelocation
        Start DOSBox using the language specified in "languagefilelocation".
        See Section 14: "The Language File" for more details.

  -machine machinetype
        Setup DOSBox to emulate a specific type of machine. Valid choices are:
        hercules, cga, ega, pcjr, tandy, svga_s3 (default) as well as
        the additional SVGA chipsets listed in the DOSBox configuration file.
        svga_s3 enables VESA emulation as well.
        For some special VGA effects the machinetype vgaonly can be used,
        note that this disables SVGA capabilities and might be slower due to the
        higher emulation precision.
        The machinetype affects the video card and the available sound cards.

  -noconsole (Windows Only)
        Start DOSBox without showing the DOSBox status window (console).
        Output will be redirected to stdout.txt and stderr.txt

  -startmapper
        Enter the keymapper directly on startup. Useful for people with
        keyboard problems.

  -noautoexec
        Skips the [autoexec] section of the loaded configuration file.

  -securemode
        Same as -noautoexec, but adds config.com -securemode at the
        bottom of AUTOEXEC.BAT (which in turn disables any changes to how
        the drives are mounted inside DOSBox).

  -scaler scaler
        Uses the scaler specified by "scaler". See the DOSBox configuration file
        for the available scalers.

  -forcescaler scaler
        Similar to the -scaler parameter, but tries to force usage of
        the specified scaler even if it might not fit.

  -version
        output version information and exit. Useful for frontends.

  -editconf program
        calls program with as first parameter the configuration file.
        You can specify this command more than once. In this case it will
        move to second program if the first one fails to start.

  -opencaptures program
        calls program with as first parameter the location of the captures
        folder.

  -printconf
        prints the location of the default configuration file.

  -resetconf
        removes the default configuration file.

  -resetmapper
        removes the mapperfile used by the default clean configuration file.

  -socket
        passes the socket number to the nullmodem emulation. See Section 9:
        "Serial Multiplayer feature."
```

Note: If a name/command/configfilelocation/languagefilelocation contains
     a space, put the whole name/command/configfilelocation/languagefilelocation
     between quotes ("command or file name"). If you need to use quotes within
     quotes (most likely with -c and mount):
     Windows and OS/2 users can use single quotes inside the double quotes.
     Other people should be able to use escaped double quotes inside the
     double quotes.
     Windows: -c "mount c 'c:\My folder with DOS games\'"
     Linux: -c "mount c \"/tmp/name with space\""

A rather unusual example, just to demonstrate what you can do (Windows):
dosbox D:\folder\file.exe -c "MOUNT Y H:\MyFolder"
  This mounts D:\folder as C:\ and runs file.exe.
  Before it does that, it will first mount H:\MyFolder as the Y drive.

In Windows, you can also drag directories/files onto the DOSBox executable.



# Internal Programs

DOSBox supports most of the DOS commands found in command.com.
To get a list of the internal commands type "HELP" at the prompt.

In addition, the following commands are available:

```
MOUNT "Emulated Drive letter" "Real Drive or Directory"
      [-t type] [-aspi] [-ioctl] [-noioctl] [-usecd number] [-size drivesize]
      [-label drivelabel] [-freesize size_in_mb]
      [-freesize size_in_kb (floppies)]
MOUNT -cd
MOUNT -u "Emulated Drive letter"

  Program to mount local directories as drives inside DOSBox.

  "Emulated Drive letter"
        The driveletter inside DOSBox (for example C).

  "Real Drive letter (usually for CD-ROMs in Windows) or Directory"
        The local directory you want accessible inside DOSBox.

  -t type
        Type of the mounted directory.
        Supported are: dir (default), floppy, cdrom.

  -size drivesize
	(experts only)
        Sets the size of the drive, where drivesize is of the form
        "bps,spc,tcl,fcl":
           bps: bytes per sector, by default 512 for regular drives and
                2048 for CD-ROM drives
           spc: sectors per cluster, usually between 1 and 127
           tcl: total clusters, between 1 and 65534
           fcl: total free clusters, between 1 and tcl

  -freesize size_in_mb | size_in_kb
        Sets the amount of free space available on a drive
        in megabytes (regular drives) or kilobytes (floppy drives).
        This is a simpler version of -size.

  -label drivelabel
        Sets the name of the drive to "drivelabel". Needed on some systems
        if the CD-ROM label isn't read correctly (useful when a program
        can't find its CD-ROM). If you don't specify a label
        and no lowlevel support is selected (that is omitting the -usecd #
        and/or -aspi parameters, or specifying -noioctl):
          For Windows: label is extracted from "Real Drive".
          For Linux: label is set to NO_LABEL.

        If you do specify a label, this label will be kept as long as the drive
        is mounted. It will not be updated !!

  -aspi
        Forces use of the aspi layer. Only valid if mounting a CD-ROM under
        Windows systems with an ASPI-Layer.

  -ioctl (automatic selection of the CD audio interface)
  -ioctl_dx (digital audio extraction used for CD audio)
  -ioctl_dio (ioctl calls used for CD audio)
  -ioctl_mci (MCI used for CD audio)
        Forces use of ioctl commands. Only valid if mounting a CD-ROM under
        a Windows OS which support them (Win2000/XP/NT).
        The various choices only differ in the way CD audio is handled,
        preferably -ioctl_dio is used (lowest workload), but this might not
        work on all systems, so -ioctl_dx (or -ioctl_mci) can be used.

  -noioctl
        Forces use of the SDL CD-ROM layer. Valid on all systems.

  -usecd number
        Valid on all systems, under Windows the -noioctl switch has to be
        present to make use of the -usecd switch.
        Enables to select the drive that should be used by SDL. Use this if
        the wrong or no CD-ROM drive is mounted while using the SDL CD-ROM
        interface. "number" can be found by "MOUNT -cd".

  -cd
        Displays all CD-ROM drives detected by SDL, and their numbers.
        See the information at the -usecd entry above.

  -u
        Removes the mount. Doesn't work for Z:\.

  Note: It's possible to mount a local directory as CD-ROM drive,
        but hardware support is then missing.

  Basically MOUNT allows you to connect real hardware to DOSBox's emulated PC.
  So MOUNT C C:\GAMES tells DOSBox to use your C:\GAMES directory as drive C:
  in DOSBox. MOUNT C E:\SomeFolder tells DOSBox to use your E:\SomeFolder
  directory as drive C: in DOSBox.

  Mounting your entire C drive with MOUNT C C:\ is NOT recommended! The same
  is true for mounting the root of any other drive, except for CD-ROMs (due to
  their read-only nature).
  Otherwise if you or DOSBox make a mistake you may lose all your files.
  Also never mount a "Windows" or "Program Files" folders or their subfolders
  in Windows Vista/7 as DOSBox may not work correctly, or will stop working
  correctly later. It is recommended to keep all your dos applications/games
  in a simple folder (for example c:\dosgames) and mount that.

  You should always install your game inside DOSBox.
  So if you have the game on CD you always (even after installation!)
  have to mount both: folder as a harddisk drive and a CD-ROM.
  HardDisk should always be mounted as c
    CD-ROM should always be mounted as d
    Floppy should always be mounted as a (or b)

  Basic MOUNT Examples for normal usage (Windows):

   1. To mount a folder as a harddisk drive:
          mount c d:\dosgames

   3. To mount your CD-ROM drive E as CD-ROM drive D in DOSBox:
          mount d e:\ -t cdrom

   2. To mount your drive a: as a floppy:
          mount a a:\ -t floppy

  Advanced MOUNT examples (Windows):

   4. To mount a hard disk drive with ~870 mb free diskspace (simple version):
          mount c d:\dosgames -freesize 870

   5. To mount a drive with ~870 mb free diskspace (experts only, full control):
          mount c d:\dosgames -size 512,127,16513,13500

   1. To mount c:\dosgames\floppy as a floppy:
          mount a c:\dosgames\floppy -t floppy


  Other MOUNT examples:

   3. To mount system CD-ROM drive at mountpoint /media/cdrom as CD-ROM drive D
      in DOSBox:
          mount d /media/cdrom -t cdrom -usecd 0

   6. To mount /home/user/dosgames as drive C in DOSBox:
          mount c /home/user/dosgames

   7. To mount the directory where DOSBox was started as C in DOSBox:
          mount c .
          (note the . which represents the directory where DOSBox was started,
          on Windows Vista/7 don't use this if you installed DOSBox
          to your "Program Files" folder)

  If you want to mount a CD image or floppy image, check IMGMOUNT.
  MOUNT also works with images but only if you use external program,
  for example (both are free):
  - Daemon Tools Lite (for CD images),
  - Virtual Floppy Drive (for floppy images).
  Although IMGMOUNT can give better compatibility.


MEM
  Program to display the amount and type of free memory.


VER
VER set major_version [minor_version]
  Display the current DOSBox version and reported DOS version
  (parameterless usage).
  Change the reported DOS version with the "set" parameter,
  for example: "VER set 6 22" to have DOSBox report DOS 6.22 as version number.


CONFIG -writeconf filelocation
CONFIG -writeconf
CONFIG -wcp filelocation
CONFIG -wcd
CONFIG -writelang filelocation
CONFIG -axadd
CONFIG -axclear
CONFIG -axtype
CONFIG -r [parameters]
CONFIG -l
CONFIG -help
CONFIG -help sections
CONFIG -help section
CONFIG -help section property
CONFIG -securemode
CONFIG -set "section property=value"
CONFIG -get "section property"

  CONFIG can be used to change or query various settings of DOSBox
  during runtime. It can save the current settings and language strings to
  disk. Information about all possible sections and properties can
  be found in Section 13: "The configuration (options) file".

  -writeconf filelocation
     (or -wc filelocation)
     Write the current configuration settings to a file in a specified location
     relative to the DOSBox config directory. Relative and absolute paths are 
     possible. "filelocation" is located on the local drive, not a mounted 
     drive in DOSBox.
     
     The configuration file controls various settings of DOSBox:
     the amount of emulated memory, the emulated sound cards and many more
     things. It allows access to AUTOEXEC.BAT as well.
     See Section 13: "The configuration (options) file" for more information.

  -writeconf
     (or -wc)
     Write the configuration to the primary loaded config file.

  -wcp filelocation
     Write the current configuration settings to the specified file in or 
     relative to the DOSBox program start directory. Realtive and absolute 
     paths are possible. This is located on a drive on the host, not a mounted
     drive in DOSBox. It is useful if you keep DOSBox on a removable media.
     If file is omitted, the configuration will be written to dosbox.conf.
     
  -wcd
     Write the current configuration to the default config file.
        
     
  -writelang filelocation
     (or -wl filelocation)
     Write the current language settings to a file in a specified location.
     "filelocation" is located on the local drive, not a mounted drive
     in DOSBox. The language file controls all visible output of the internal
     commands and the internal DOS.
     See Section 14: "The Language File" for more information.

  -axadd "line1" "line2" ...
     Adds a command line to the autoexec section.

  -axclear
     Clears the autoexec section.
     
  -axtype
     Prints the content of the autoexec section.

  -r [parameters]
     Restart DOSBox, either with the parameters that were used to start the
     current instance or any that are appended.

  -l 
     lists DOSBox parameters:
     - the configuration directory
     - the config files that were used when starting this session
     - the command line parameters DOSBox was started with

  -h, -help, -?
     Displays an overvie of the config commands.

  -h, -help, -? sections
     Displays the list of sections in the config file.
  
  -h, -help, -? section
     Displays the list of properties contained in the specified section.
     
  -h, -help, -? section property
     Shows information about the specified property in the specified section:
     - purpose of the property
     - possible values, current value, default value
     - wether it can definitely not be changed at runtime
       
  -securemode
     Switches DOSBox to a more secure mode. In this mode the internal
     commands MOUNT, IMGMOUNT and BOOT won't work. It's not possible either
     to create a new configfile or languagefile in this mode.
     (Warning: you can only undo this mode by restarting DOSBox.)

  -set "section property=value"
     CONFIG will attempt to set the property to new value.

  -get "section property"
     The current value of the property is reported and stored in the
     environment variable %CONFIG%. This can be used to store the value
     when using batch files.

  Both "-set" and "-get" work from batch files and can be used to set up your
  own preferences for each game. Although it may be easier to use separate
  DOSBox's configuration files for each game instead.

  Examples:
    1. To create a configuration file in your c:\dosgames directory:
        config -writeconf c:\dosgames\dosbox.conf
    2. To set the cpu cycles to 10000:
        config -set "cpu cycles=10000"
    3. To turn EMS memory emulation off:
        config -set "dos ems=false"
    4. To check which cpu core is being used.
        config -get "cpu core"
    5. To view the list of possible cpu cores:
        config -help cpu core
    6. To change the machine type and restart:
        config -set "machine cga"
        config -wc -r
    7. To configure the autoexec section to auto-mount a directory at start:
        config -axadd "mount c c:\dosgames" "c:"
        config -wc
    8. To create a specific config file in the config directory:
        config -set "dos ems=false"
        config -set "cpu cycles=10000"
        config -set "core dynamic"
        config -axadd "mount c c:\dosgames" "c:" "cd my_game" "my_game"
        config -wc my_config.conf
    9. To restart DOSBox from a specific config file in the config directory:
        config -r -conf my_config.conf

LOADFIX [-size] [program] [program-parameters]
LOADFIX -f
  Program to reduce the amount of available conventional memory.
  Useful for old programs which don't expect much memory to be free.

  -size
        number of kilobytes to "eat up", default = 64kb

  -f
        frees all previously allocated memory

  Examples:
    1. To start mm2.exe and allocate 64kb memory
       (mm2 will have 64 kb less available):
       loadfix mm2
    2. To start mm2.exe and allocate 32kb memory:
       loadfix -32 mm2
    3. To free previous allocated memory:
       loadfix -f


RESCAN [Drive:] [-All]
  Make DOSBox reread the directory structure. Useful if you changed something
  on a mounted drive outside of DOSBox. (CTRL - F4 does this as well!)
  
  Drive: 
        Drive to refresh.

  -All
        Refresh all drives.

  if both a Drive: and -All are missing, then the current drive will be 
  refreshed.
  

MIXER
  Makes DOSBox display its current volume settings.
  Here's how you can change them:

  mixer channel left:right [/NOSHOW] [/LISTMIDI]

  channel
     Can be one of the following: MASTER, DISNEY, SPKR, GUS, SB, FM [, CDAUDIO].
     CDAUDIO is only available if a CD-ROM interface with volume control is
     enabled (CD image, ioctl_dx).

  left:right
     The volume levels in percentages. If you put a D in front it will be
     in decibel (Example: mixer gus d-10).

  /NOSHOW
     Prevents DOSBox from showing the result if you set one
     of the volume levels.

  /LISTMIDI
     In Windows lists the available midi devices on your PC. To select a device
     other than the Windows default midi-mapper, change the line 'midiconfig='
     in the [midi] section of the configuration file to 'midiconfig=id', where
     'id' is the number for the device as listed by LISTMIDI. eg. midiconfig=2

     In Linux this option doesn't work, but you get similar results by using
     'pmidi -l' in console. Then change the line 'midiconfig=' to 
     'midiconfig=port', where 'port' is the port for the device as listed by 
     'pmidi -l'. eg. midiconfig=128:0


IMGMOUNT
  A utility to mount disk images and CD-ROM images in DOSBox.

  IMGMOUNT DRIVE [imagefile] -t [image_type] -fs [image_format]
            -size [sectorsbytesize, sectorsperhead, heads, cylinders]
  IMGMOUNT DRIVE [imagefile1 imagefile2 .. imagefileN] -t cdrom -fs iso

  imagefile
      Location of the image file to mount in DOSBox. The location can be
      on a mounted drive inside DOSBox, or on your real disk. It is possible
      to mount CD-ROM images (ISOs or CUE/BIN or CUE/IMG) too.
      If you need CD swapping capabilities, specify all images in succession
      (see the next entry).
      CUE/BIN pairs and cue/img are the preferred CD-ROM image types as they can
      store audio tracks compared to ISOs (which are data-only). For
      the CUE/BIN mounting always specify the CUE sheet.

  imagefile1 imagefile2 .. imagefileN
      Location of the image files to mount in DOSBox. Specifying a number
      of image files is only allowed for CD-ROM images.
      The CD's can be swapped with CTRL-F4 at any time.
      This is required for games which use multiple CD-ROMs and require the CD
      to be switched during the gameplay at some point.

  -t
      The following are valid image types:
        floppy: Specifies a floppy image. DOSBox will automatically identify
                the disk geometry (360K, 1.2MB, 720K, 1.44MB, etc).
        cdrom:  Specifies a CD-ROM image. The geometry is automatic and
                set for this size. This can be an iso or a cue/bin pair or
                a cue/img pair.
        hdd:    Specifies a harddrive image. The proper CHS geometry must be set
                for this to work.

  -fs
      The following are valid file system formats:
        iso:  Specifies the ISO 9660 CD-ROM format.
        fat:  Specifies that the image uses the FAT file system. DOSBox will
              attempt to mount this image as a drive in DOSBox and make
              the files available from inside DOSBox.
        none: DOSBox will make no attempt to read the file system on the disk.
              This is useful if you need to format it or if you want to boot
              the disk using the BOOT command. When using the "none"
              filesystem, you must specify the drive number (2 or 3,
              where 2 = master, 3 = slave) rather than a drive letter.
              For example, to mount a 70MB image as the slave drive device,
              you would type (without the quotes):
                "imgmount 3 d:\test.img -size 512,63,16,142 -fs none"
                Compare this with a mount to be able to access the drive
                within DOSBox, which would read as:
                "imgmount e: d:\test.img -size 512,63,16,142"

  -size
     The Cylinders, Heads and Sectors of the drive.
     Required to mount hard drive images.

  An example how to mount CD-ROM images (in Linux):
    1. imgmount d /tmp/cdimage1.cue /tmp/cdimage2.cue -t cdrom
  or (which also works):
    2a. mount c /tmp
    2b. imgmount d c:\cdimage1.cue c:\cdimage2.cue -t cdrom
  (in Windows):
    imgmount d f:\img\CD1.cue f:\img\CD2.cue f:\img\CD3.cue -t cdrom
    imgmount d "g:\img\7th Guest CD1.cue" "g:\img\7th Guest CD2.cue" -t cdrom
  Don't forget that you can also use MOUNT with images, but only if you use
  external program, for example (both are free):
  - Daemon Tools Lite (for CD images),
  - Virtual Floppy Drive (for floppy images).
  Although IMGMOUNT can give better compatibility.


BOOT
  Boot will start floppy images or hard disk images independent of
  the operating system emulation offered by DOSBox. This will allow you to
  play booter floppies or boot other operating systems inside DOSBox.
  If the target emulated system is PCjr (machine=pcjr) the boot command
  can be used to load PCjr cartridges (.jrc). 

  BOOT [diskimg1.img diskimg2.img .. diskimgN.img] [-l driveletter]
  BOOT [cart.jrc]  (PCjr only)

  diskimg1.img diskimg2.img .. diskimgN.img
     This can be any number of floppy disk images one wants mounted after
     DOSBox boots the specified drive letter.
     To swap between images, hit CTRL-F4 to change from the current disk
     to the next disk in the list. The list will loop back from the last
     disk image to the beginning.

  [-l driveletter]
     This parameter allows you to specify the drive to boot from.
     The default is the A drive, the floppy drive. You can also boot
     a hard drive image mounted as master by specifying "-l C"
     without the quotes, or the drive as slave by specifying "-l D"

   cart.jrc (PCjr only)
     When emulation of a PCjr is enabled, cartridges can be loaded with
     the BOOT command. Support is still limited.


IPX

  You need to enable IPX networking in the configuration file of DOSBox.

  All of the IPX networking is managed through the internal DOSBox program
  IPXNET. For help on the IPX networking from inside DOSBox, type
  "IPXNET HELP" (without quotes) and the program will list the commands
  and relevant documentation.

  With regard to actually setting up a network, one system needs to be
  the server. To set this up, type "IPXNET STARTSERVER" (without the quotes)
  in a DOSBox session. The server DOSBox session will automatically add
  itself to the virtual IPX network. For every additional computer that
  should be part of the virtual IPX network, you'll need to type
  "IPXNET CONNECT <computer host name or IP>".
  For example, if your server is at bob.dosbox.com, you would type
  "IPXNET CONNECT bob.dosbox.com" on every non-server system.

  To play games that need Netbios a file named NETBIOS.EXE from Novell is
  needed. Establish the IPX connection as explained above, then run
  "netbios.exe".

  The following is an IPXNET command reference:

  IPXNET CONNECT

     IPXNET CONNECT opens a connection to an IPX tunneling server
     running on another DOSBox session. The "address" parameter specifies
     the IP address or host name of the server computer. You can also
     specify the UDP port to use. By default IPXNET uses port 213 - the
     assigned IANA port for IPX tunneling - for its connection.

     The syntax for IPXNET CONNECT is:
     IPXNET CONNECT address <port>

  IPXNET DISCONNECT

     IPXNET DISCONNECT closes the connection to the IPX tunneling server.

     The syntax for IPXNET DISCONNECT is:
     IPXNET DISCONNECT

  IPXNET STARTSERVER

     IPXNET STARTSERVER starts an IPX tunneling server on this DOSBox
     session. By default, the server will accept connections on UDP port
     213, though this can be changed. Once the server is started, DOSBox
     will automatically start a client connection to the IPX tunneling server.

     The syntax for IPXNET STARTSERVER is:
     IPXNET STARTSERVER <port>

     If the server is behind a router, UDP port <port> needs to be forwarded
     to that computer.

     On Linux/Unix-based systems port numbers smaller than 1023 can only be
     used with root privileges. Use ports greater than 1023 on those systems.

  IPXNET STOPSERVER

     IPXNET STOPSERVER stops the IPX tunneling server running on this DOSBox
     session. Care should be taken to ensure that all other connections have
     terminated as well, since stopping the server may cause lockups on other
     machines that are still using the IPX tunneling server.

     The syntax for IPXNET STOPSERVER is:
     IPXNET STOPSERVER

  IPXNET PING

     IPXNET PING broadcasts a ping request through the IPX tunneled network.
     In response, all other connected computers will respond to the ping
     and report the time it took to receive and send the ping message.

     The syntax for IPXNET PING is:
     IPXNET PING

  IPXNET STATUS

     IPXNET STATUS reports the current state of this DOSBox session's
     IPX tunneling network. For a list of all computers connected to the
     network use the IPXNET PING command.

     The syntax for IPXNET STATUS is:
     IPXNET STATUS


KEYB [keyboardlayoutcode [codepage [codepagefile]]]

  Change the keyboard layout. For detailed information about keyboard layouts
  please see Section 8: "Keyboard Layout".

  [keyboardlayoutcode] is a string consisting of five or less characters,
     examples are PL214 (Polish typists) or PL457 (Polish programmers).
     It specifies the keyboard layout to be used.
     The list of all layouts built into DOSBox is here:
     https://www.vogons.org/viewtopic.php?t=21824

  [codepage] is the number of the codepage to be used. The keyboard layout
     has to provide support for the specified codepage, otherwise the layout
     loading will fail.
     If no codepage is specified, an appropriate codepage for the requested
     layout is chosen automatically.

  [codepagefile] can be used to load codepages that are yet not compiled
     into DOSBox. This is only needed when DOSBox does not find the codepage.
     If no codepagefile is specified, but you place all ten ega.cpx files
     (from FreeDOS) in the DOSBox program folder, an appropriate codepagefile
     for the requested layout/codepage is chosen automatically.

  Examples:
    1. To load the polish typist keys layout (automatically uses codepage 852):
         keyb pl214
    2. To load one of russian keyboard layouts with codepage 866:
         keyb ru441 866
       In order to type russian characters press ALT+RIGHT-SHIFT.
    3. To load one of french keyboard layouts with codepage 850 (where the
       codepage is defined in EGACPI.DAT):
         keyb fr189 850 EGACPI.DAT
    4. To load codepage 858 (without a keyboard layout):
         keyb none 858
       This can be used to change the codepage for the FreeDOS keyb2 utility.
    5. To display the current codepage and, if loaded, the keyboard layout:
         keyb
```


For more information use the /? command line switch with the programs.



# Special Keys

```
ALT-ENTER     Switch to full screen and back.
ALT-PAUSE     Pause emulation (hit ALT-PAUSE again to continue).
CTRL-F1       Start the keymapper.
CTRL-F4       Change between mounted floppy/CD images. Update directory cache 
              for all drives.
CTRL-ALT-F5   Start/Stop creating a movie of the screen. (avi video capturing)
CTRL-F5       Save a screenshot. (PNG format)
CTRL-F6       Start/Stop recording sound output to a wave file.
CTRL-ALT-F7   Start/Stop recording of OPL commands. (DRO format)
CTRL-ALT-F8   Start/Stop the recording of raw MIDI commands.
CTRL-F7       Decrease frameskip.
CTRL-F8       Increase frameskip.
CTRL-F9       Kill DOSBox.
CTRL-F10      Capture/Release the mouse.
CTRL-F11      Slow down emulation (Decrease DOSBox Cycles).
CTRL-F12      Speed up emulation (Increase DOSBox Cycles)*.
ALT-F12       Unlock speed (turbo button/fast forward)**.
CTRL-ALT-HOME Restart DOSBox.
F11, ALT-F11  (machine=cga) change tint in NTSC output modes***.
F11           (machine=hercules) cycle through amber, green, white colouring***.
```

*NOTE: Once you increase your DOSBox cycles beyond your computer CPU resources,
       it will produce the same effect as slowing down the emulation.
       This maximum will vary from computer to computer.

**NOTE: You need free CPU resources for this (the more you have, the faster
        it goes), so it won't work at all with cycles=max or a too high amount
        of fixed cycles. You have to keep the keys pressed for it to work!

***NOTE: These keys won't work if you saved a mapper file earlier with
         a different machine type. So either reassign them or reset the mapper.

These are the default keybindings. They can be changed in the keymapper
(see Section 7: "KeyMapper"). 

In Mac OS X you can try using cmd(applekey) together with Ctrl (and/or ) Fn, 
if the key doesn't work i.e. fn-cmd-ctrl-F1, but some keys may still need 
remapping (in Linux too).

Saved/recorded files can be found in:
   (Windows)    "Start/WinLogo Menu"->"All Programs"->"DOSBox-0.74-2"->Extras
   (Linux)      ~/.dosbox/capture
   (Mac OS X)   "~/Library/Preferences/capture"
This can be changed in the DOSBox configuration file.



# Joystick/Gamepad

The standard joystick port in DOS supports a maximum of 4 axes and 4 buttons.
For more, different modifications of that configuration were used.

To force DOSBox to use a different type of emulated joystick/gamepad, the entry
"joysticktype" in the [joystick] section of the DOSBox configuration file can
be used.

```
none  - disables controller support.
auto  - (default) autodetects whether you have one or two controllers connected:
          if you have one - '4axis' setting is used,
          if you have two - '2axis' setting is used.
2axis - If you have two controllers connected, each will emulate a joystick
        with 2 axes and 2 buttons. If you have only one controller connected,
        it will emulate a joystick with only 2 axis and 2 buttons.
4axis - supports only first controller, emulates a joystick
        with 4 axis and 4 buttons or a gamepad with 2axis and 6 buttons.
4axis_2 - supports only second controller.
fcs   - supports only first controller, emulates ThrustMaster
        Flight Control System, with 3-axes, 4 buttons and 1 hat.
ch    - supports only first controller, emulates CH Flightstick,
        with 4-axes, 6 buttons and 1 hat, but you cannot press more
        than one button at the same time.
```

You also have to configure controller properly inside the game.
It is important to remember that if you saved the mapperfile without joystick
connected, or with a different joystick setting, your new setting will not work
properly, or not work at all, until you reset DOSBox's mapperfile.

If controller is working properly outside DOSBox, but doesn't calibrate properly
inside DOSBox, try a different 'timed' setting in DOSBox's configuration file.



# KeyMapper

Start the DOSBox mapper either with CTRL-F1 (see Section 5: "Special Keys") or
-startmapper (see Section 3: "Command Line Parameters"). 
You are presented with a virtual keyboard and a virtual joystick.

These virtual devices correspond to the keys and events DOSBox will
report to the DOS applications. If you click on a button with your mouse,
you can see in the lower left corner with which event it is associated
(EVENT) and to what events it is currently bound.

```
Event: EVENT
BIND: BIND (the real key/button/axis you push with your finger/hand)

                                    Add   Del
mod1  hold                                Next
mod2
mod3


EVENT
    The key or joystick axis/button/hat DOSBox will report to DOS applications.
    (the event that will happen during the game, (eg. shooting/jumping/walking)
BIND
    The key on your real keyboard or the axis/button/hat on your real
    joystick(s) (as reported by SDL), which is connected to the EVENT.
mod1,2,3
    Modifiers. These are keys you need to have to be pressed while pressing
    BIND. mod1 = CTRL and mod2 = ALT. These are generally only used when you
    want to change the special keys of DOSBox.
Add
    Add a new BIND to this EVENT. Basically add a key from your keyboard or an
    event from the joystick (button press, axis/hat movement) which will
    produce the EVENT in DOSBox.
Del
    Delete the BIND to this EVENT. If an EVENT has no BINDS, then it is not
    possible to trigger this event in DOSBox (that is there's no way to type
    the key or use the respective action of the joystick).
Next
    Go through the list of bindings which map to this EVENT.


Example:
Q1. You want to have the X on your keyboard to type a Z in DOSBox.
    A. Click on the Z on the keyboard mapper. Click "Add".
       Now press the X key on your keyboard.

Q2. If you click "Next" a couple of times, you will notice that the Z on your
    keyboard also produces an Z in DOSBox.
    A. Therefore select the Z again, and click "Next" until you have the Z on
       your keyboard. Now click "Del".

Q3. If you try it out in DOSBox, you will notice that pressing X makes ZX
    appear.
     A. The X on your keyboard is still mapped to the X as well! Click on
        the X in the keyboard mapper and search with "Next" until you find the
        mapped key X. Click "Del".
```

Examples of remapping the joystick:
  You have a joystick attached, it is working fine under DOSBox and you
  want to play some keyboard-only game with the joystick (it is assumed
  that the game is controlled by the arrows on the keyboard):
    1. Start the mapper, then click on one of the left keyboard arrow.
       EVENT should be key_left. Now click on Add and move your joystick
       in the respective direction, this should add an event to the BIND.
    2. Repeat the above for the missing three directions, additionally
       the buttons of the joystick can be remapped as well (fire/jump).
    3. Click on Save, then on Exit and test it with some game.

  You want to swap the y-axis of the joystick because some flightsim uses
  the up/down joystick movement in a way you don't like, and it is not
  configurable in the game itself:
    1. Start the mapper and click on Y- in the first joystick field.
       EVENT should be jaxis_0_1-.
    2. Click on Del to remove the current binding, then click Add and move
       your joystick downwards. A new bind should be created.
    3. Repeat this for Y+, save the layout and finally test it with some game.

  If you want to remap anything to your d-pad/hat you will have to change
  'joysticktype=auto' to 'joysticktype=fcs' in configuration file. Maybe this
  will be improved in the next DOSBox version.


If you change the default mapping, you can save your changes by clicking on
"Save". DOSBox will save the mapping to a location specified in
the configuration file (the mapperfile= entry). At startup, DOSBox will load
your mapperfile, if it is present in the DOSBox configuration file.



# Keyboard Layout

To switch to a different keyboard layout, either the entry "keyboardlayout"
in the [dos] section of the DOSBox configuration file or the internal DOSBox
program keyb.com (Section 4: "Internal Programs") can be used. Both accept
DOS conforming language codes (see below), but only by using keyb.com a
custom codepage can be specified.

The default keyboardlayout=auto currently works under Windows only. The language
is chosen according to the OS language, but the keyboard layout is not detected.

Layout switching
  DOSBox supports a number of keyboard layouts and codepages by default,
  in this case just the layout identifier needs to be specified (like
  keyboardlayout=PL214 in the DOSBox configuration file, or using "keyb PL214"
  at the DOSBox command prompt). The list of all layouts built into DOSBox is
  here: https://www.vogons.org/viewtopic.php?t=21824

  Some keyboard layouts (for example layout GK319 codepage 869 and layout RU441
  codepage 808) have support for dual layouts that can be accessed by pressing
  LeftALT+RrightSHIFT for one layout and LeftALT+LeftSHIFT for the other.
  Some keyboard layouts (for example layout LT456 codepage 771) have support
  for three layouts, third can be accessed by pressing LeftALT+LeftCTRL

Supported external files
  The FreeDOS .kl files are supported (FreeDOS keyb2 keyboard layoutfiles) as
  well as the FreeDOS keyboard.sys/keybrd2.sys/keybrd3.sys libraries which
  consist of all available .kl files.
  See http://www.freedos.org/ for precompiled keyboard layouts if
  the DOSBox-integrated layouts don't work for some reason, or if updated or
  new layouts become available.

  Both .CPI (MS-DOS and compatible codepage files) and .CPX (FreeDOS
  UPX-compressed codepage files) can be used. Some codepages are compiled
  into DOSBox, so it is mostly not needed to care about external codepage
  files. If you need a different (or custom) codepage file, copy it into
  the directory of the DOSBox so it is accessible for DOSBox.
  If you place all ten ega.cpx files (from FreeDOS) in DOSBox folder,
  an appropriate codepagefile for the requested layout/codepage is
  chosen automatically.

  Additional layouts can be added by copying the corresponding .kl file into
  the directory of the DOSBox configuration file and using the first part of
  the filename as language code.
  Example: For the file UZ.KL (keyboard layout for Uzbekistan) specify
           "keyboardlayout=uz" in the DOSBox configuration file.
  The integration of keyboard layout packages (like keybrd2.sys) works similar.

Note that the keyboard layout allows foreign characters to be entered, but
there is NO support for them in filenames. Try to avoid them both inside
DOSBox as well as in files on your host operating system that are accessible
by DOSBox.



# Serial Multiplayer feature

DOSBox can emulate a serial nullmodem cable over network and internet.
It can be configured through the [serialports] section in the DOSBox
configuration file.

To create a nullmodem connection, one side needs to act as the server and
one as the client.

The server needs to be set up in the DOSBox configuration file like this:
   serial1=nullmodem

The client:
   serial1=nullmodem server:<IP or name of the server>

Now start your game and choose nullmodem / serial cable / already connected
as multiplayer method on COM1. Set the same baudrate on both computers.

Furthermore, additional parameters can be specified to control the behavior
of the nullmodem connection. These are all parameters:

```
 * port:         - TCP port number. Default: 23
 * rxdelay:      - how long (milliseconds) to delay received data if the
                   interface is not ready. Increase this value if you encounter
                   overrun errors in the DOSBox status window. Default: 100
 * txdelay:      - how long to gather data before sending a packet. Default: 12
                   (reduces Network overhead)
 * server:       - This nullmodem will be a client connecting to the specified
                   server. (No server argument: be a server.)
 * transparent:1 - Only send the serial data, no RTS/DTR handshake. Use this
                   when connecting to anything other than a nullmodem.
 * telnet:1      - Interpret Telnet data from the remote site. Automatically
                   sets transparent.
 * usedtr:1      - The connection will not be established until DTR is switched
                   on by the DOS program. Useful for modem terminals.
                   Automatically sets transparent.
 * inhsocket:1   - Use a socket passed to DOSBox by command line. Automatically
                   sets transparent. (Socket Inheritance: It is used for
                   playing old DOS door games on new BBS software.)
```

Example: Be a server listening on TCP port 5000.
   serial1=nullmodem server:<IP or name of the server> port:5000 rxdelay:1000



# How to speed up/slow down DOSBox

DOSBox emulates the CPU, the sound and graphic cards, and other peripherals
of a PC, all at the same time. The speed of an emulated DOS application
depends on how many instructions can be emulated, which is adjustable
(number of cycles).

## CPU Cycles (speed up/slow down)
  By default (cycles=auto) DOSBox tries to detect whether a game needs to
  be run with as many instructions emulated per time interval as possible
  (cycles=max, sometimes this results in game working too fast or unstable),
  or whether to use fixed amount of cycles (cycles=3000, sometimes this results
  in game working too slow or too fast). But you can always manually force
  a different setting in the DOSBox's configuration file.

  You can force the slow or fast behavior by setting a fixed amount of cycles
  in the DOSBox's configuration file. If you set for example cycles=10000, the
  DOSBox window will display a line "CPU speed: fixed 10000 cycles" at the top.
  In this mode you can reduce the amount of cycles even more by hitting CTRL-F11
  (you can go as low as you want) or raise it by hitting CTRL-F12 as much as you
  want, but you will be limited by the power of one core of your computer's CPU.
  You can see how much free time your real CPU's cores have by looking at
  the Task Manager in Windows 2000/XP/Vista/7 and the System Monitor
  in Windows 95/98/ME. Once 100% of the power of your computer's real CPU's one
  core is used, there is no further way to speed up DOSBox (it will actually
  start to slow down), unless you reduce the load generated by the non-CPU parts
  of DOSBox. DOSBox can use only one core of your CPU, so If you have
  for example a CPU with 4 cores, DOSBox will not be able to use the power
  of three other cores.

  You can also force the fast behavior by setting cycles=max in the DOSBox
  configuration file. The DOSBox window will display a line
  "CPU speed: max 100% cycles" at the top then. This time you won't have to care
  how much free time your real CPU cores have, because DOSBox will always use
  100% of your real CPU's one core. In this mode you can reduce the amount
  of your real CPU's core usage by CTRL-F11 or raise it with CTRL-F12.

## CPU Core (speed up)
  On x86 architectures you can try to force the usage of a dynamically
  recompiling core (set core=dynamic in the DOSBox configuration file).
  This usually gives better results if the auto detection (core=auto) fails.
  It is best accompanied by cycles=max. But you may also try using it with
  high amounts of cycles (for example 20000 or more). Note that there might be
  games that work worse/crash with the dynamic core (so save your game often),
  or do not work at all!

## Graphics emulation (speed up)
  VGA emulation is a demanding part of DOSBox in terms of actual CPU usage.
  Increase the number of frames skipped (in increments of one) by pressing
  CTRL-F8. Your CPU usage should decrease when using a fixed cycle setting,
  and you will be able to increase cycles with CTRL-F12.
  You can repeat this until the game runs fast enough for you.
  Please note that this is a trade-off: you lose in fluidity of video what
  you gain in speed.

## Sound emulation (speed up)
  You can also try to disable the sound through the setup utility of the game
  to reduce load on your CPU further. Setting nosound=true in DOSBox's
  configuration does NOT disable the emulation of sound devices, just
  the output of sound will be disabled.

Also try to close every program but DOSBox to reserve as much resources
as possible for DOSBox.


Advanced cycles configuration:
The cycles=auto and cycles=max settings can be parameterized to have
different startup defaults. The syntax is
  cycles=auto ["realmode default"] ["protected mode default"%]
              [limit "cycle limit"]
  cycles=max ["protected mode default"%] [limit "cycle limit"]
Example:
  cycles=auto 5000 80% limit 20000
  will use cycles=5000 for real mode games, 80% CPU throttling for
  protected mode games along with a hard cycle limit of 20000



# Troubleshooting

General tip:
  Check messages in the DOSBox status window. See Section 12: "DOSBox Status Window".

DOSBox crashes right after starting it:
  - use different values for the output= entry in your DOSBox
    configuration file
  - try to update your graphics card driver and DirectX
  - (Linux) set the environment variable SDL_AUDIODRIVER to alsa or oss.

Running a certain game closes DOSBox, crashes with some message or hangs:
  - see if it works with a default DOSBox installation
    (unmodified configuration file)
  - try it with sound disabled (use the sound configuration
    program that comes with the game, additionally you can
    set sbtype=none and gus=false in the DOSBox configuration file)
  - change some entries of the DOSBox configuration file, especially try:
      core=normal
      fixed cycles (for example cycles=10000)
      ems=false
      xms=false
    or combinations of the above settings,
    similar the machine settings that control the emulated chipset and
    functionality:
      machine=vesa_nolfb
    or
      machine=vgaonly
  - use loadfix before starting the game

The game exits to the DOSBox prompt with some error message:
  - read the error message closely and try to locate the error
  - try the hints at the above sections
  - mount differently as some games are picky about the locations,
    for example if you used "mount d d:\oldgames\game" try
    "mount c d:\oldgames\game" and "mount c d:\oldgames"
  - if the game requires a CD-ROM be sure you used "-t cdrom" when
    mounting and try different additional parameters (the ioctl,
    usecd and label switches, see the appropriate section)
  - check the file permissions of the game files (remove read-only
    attributes, add write permissions etc.)
  - try reinstalling the game within DOSBox



# DOSBox Status Window

DOSBox's status window contains useful information about your current
configuration, your actions in DOSBox, errors which occurred and more.
Check these messages in case you encounter any problems with DOSBox.

To display the DOSBox status window:
  (Windows)  The status window is being started together with main DOSBox window.
  (Linux)    You may have to start DOSBox from a console to see the status window.
  (Mac OS X) Right click on DOSBox.app, choose "Show Package Contents"->
             ->enter "Contents"->enter "MacOS"->run "DOSBox"



# The configuration (options) file

The configuration file is automatically created the first time you run DOSBox.
The file can be found in:
   (Windows)  "Start/WinLogo Menu"->"All Programs"->"DOSBox-0.74-2"->Options
   (Linux)    ~/.dosbox/dosbox-0.74-2.conf
   (Mac OS X) "~/Library/Preferences/DOSBox 0.74-2 Preferences"
The file is divided into several sections. Each section starts with a
[section name] line. The settings are the property=value lines where value can
be altered to customize DOSBox.
\# and % indicate comment-lines.


An extra configuration file can be generated by CONFIG.COM, which can be found
on the internal DOSBox Z: drive when you start up DOSBox. Look in the Section 4:
"Internal programs" for usage of CONFIG.COM. You can start DOSBox with
the -conf switch to load the generated file and use its settings.

/DOSBox will load configuration files that are specified with -conf. If none were
specified, it will try to load "dosbox.conf" from the local directory.
If there is none, DOSBox will load the user configuration file.
This file will be created if it doesn't exist.

Important!: In Windows Vista/7 the configuration file won't work correctly
if it is located in "Windows" or "Program Files" folder or their subfolders,
or directly on c:\, so the best place for storing extra configuration files is
for example: C:\oldgames



# The Language File

A language file can be generated by CONFIG.COM, which can be found on the
internal DOSBox Z: drive when you start up DOSBox. Look in the Section 4:
"Internal programs" for usage of CONFIG.COM.
Read the language file, and you will hopefully understand how to change it.
Start DOSBox with the -lang switch to use your new language file.
Alternatively, you can setup the filename in the configuration file
in the [dosbox] section. There's a language= entry that can be changed with
the filelocation.



# Building your own version of DOSBox

Building from source available on GitHub:

```
git clone https://github.com/dmikushin/dosbox.git
cd dosbox
mkdir build
cd build
cmake ..
make -j12
```

Check the INSTALL file to see the dependencies and instructions on legacy configuration script.

# Special thanks

See the THANKS file.



# Contact

See the site: 
https://www.dosbox.com
for an email address (The Crew-page).


