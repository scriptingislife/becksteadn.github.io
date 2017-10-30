---
layout: post
title: Breaking into Windows with Sticky Keys
published: true
---
The good ol' sticky keys exploit. As a security centric IT intern, I was elected to break into the old machine an employee had forgotten the password to. The machine ran XP - easy. This exploit works on at least XP and Windows 7. The only thing required is a bootable Linux live USB and a handful of Linux and Windows commands. If all goes smoothly, you can expect to be back in the machine in roughly 5 minutes.
#What
The sticky keys prompt is titled 'sethc.exe'. This application is called by name when shift is pressed 5 times. By changing the contents of this executable, we can run whatever code we want at system level. Since this hotkey is listening even before login, it's very useful for resetting a lost password. By changing the command prompt's name to sethc.exe, we gain a system level shell which can manage users.

#How

##Disclaimer

This should be used to get into a locked computer that you have permission to unlock. If you do use it maliciously, there's plenty of tutorials online, don't say you got it from here.

##Linux

Any Linux live USB will work as long as it can mount and read the Windows file system. I usually use Ubuntu desktop. Make sure it's the same architecture as the target system. I had to reimage my USB since the host was 32 bit. By using a live USB, we can mount our Windows drive and circumvent any Windows security and have full access to any unencrypted files.

To create a live USB from a .iso image use Rufus (download below) or another image writing program.

After booting into Linux, use the file explorer to navigate to (and thereby mount) the Windows disk or mount it via the command line. Make note of the gibberish drive name.

Open a terminal and navigate to the Windows System32 directory. For Ubuntu this will be at:

`/media/Ubuntu/DRIVENAME/WINDOWS/SYSTEM32`

Make a backup of the sticky keys program with the command

`cp sethc.exe bk_sethc.exe`
    
Copy the command prompt in place of the sticky keys program with

`cp cmd.exe sethc.exe`

Reboot and boot into Windows.

###Windows

At the login screen press shift 5 times. Instead of the sticky keys prompt, a shell should appear with system32 as the current directory. Now we can create a new administrator or change the password of a current user. The 'net' set of commands is how we can manage users.

To create a new administrator

```
net user /add newaccount newpassword

net localgroup administrators newaccount /add
```

To change the password of an existing user

`net user myaccount newpassword`

Make the user an administrator if you'd like

`net localgroup administrators myaccount /add`


Now log in as the user. Press shift 5 times again and reset the computer to its normal state.

`mv bk_sethc.exe sethc.exe`

If you'd like to delete the administrator account you made to get in, use the command

`net user myaccount /delete`

You're all set. The machine should be back to normal settings and you can make any pther changes inside Windows.

#References

[Rufus Download]("https://rufus.akeo.ie/)

[Ubuntu Desktop Download]("https://www.ubuntu.com/download/desktop")

[TechNet - Net User Command]("https://technet.microsoft.com/en-us/library/bb490718.aspx")
