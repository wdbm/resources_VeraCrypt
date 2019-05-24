# VeraCrypt

# create VeraCrypt container in terminal

```Bash
veracrypt --text --create --volume-type=normal --encryption=AES --hash=SHA-256 --size=50M --filesystem=ext4 --keyfiles="" --pim=0 --password=12345 myvolumefile
```

The PIM (Personal Iterations Multiplier -- a number that most be stored along with the passcode) can be left as none. The keyfile path can be left as none. Some keystrokes are taken as a source of randomness and the volume is then created.

# mount in terminal

```Bash
veracrypt --text myvolumefile /media/veracrypt1
```

# dismount in terminal

```Bash
veracrypt --dismount myvolumefile
```

# dismount all in terminal

```Bash
veracrypt -d
```

# dismount blocked

Sometimes a process like `git-credential` can block unmounting of VeraCrypt. This can be found by running a command like the following, not requiring `sudo`:

```Bash
fuser -mv /media/veracrypt2/
                     USER PID ACCESS COMMAND
/media/veracrypt2:   root     kernel mount /media/veracrypt2
                     wbm       21711 ..c.. git-credential-
```

The process can be killed and then VeraCrypt can unmount the drive.

# force dismount

```Bash
veracrypt --dismount --force
```

## plausible deniability, hidden volumes

In "plausible deniability", hidden volumes are created in an outer volume.  Which volume is accessed depends solely on the authentication used. If the authentication for the outer volume are used, no information about the existence of the hidden volume is exposed. Without knowledge of the authentication of the hidden volume, its existence can remain unexposed.

## create hidden volume

- VeraCrypt
- "Create Volume"
- "Create an encrypted file container"
- "Hidden VeraCrypt volume"
- Create the outer VeraCrypt volume.
- In the outer volume should be some sensitive-looking files that you do not want actually to hide. These files are there for anyone coercing you to disclose a passcode. This passcode is the one you reveal and it gives access to the outer volume, not the hidden one. The idea is that the outer volume remains unedited in order to prevent damage to the inner volume.
- VeraCrypt analyses the copied "sensitive files" and the maximum size of the hidden volume is determined.
- Ensure that enemies do not have access to copies of the file over time if the hidden volume is edited over time.
- Ensure that the outer volume is mounted by VeraCrypt in such a way that writing to it is prevented. To do this, when mounting it, select "Mount Options..." and then select "Protect hidden volume against damage caused by writing to outer volume".

# TrueCrypt mode

- Tools, Preferences, Mount Options, TrueCrypt Mode

# setup 2018-02-18T0517Z

```Bash
mkdir veracrypt
cd veracrypt
wget https://launchpadlibrarian.net/289850375/veracrypt-1.19-setup.tar.bz2
tar -xvf veracrypt-1.19-setup.tar.bz2 -C veracrypt
sudo ./veracrypt-1.19-setup-gui-x64
#sudo ./veracrypt-1.19-setup-console-x86
cd ..
rm -rf veracrypt
```

# compile (not working) 2018-02-18T0517Z

```Bash
sudo apt install libfuse-dev libwxgtk2.8-dev libwxgtk2.8-dbg
mkdir tmp
cd tmp
wget https://launchpadlibrarian.net/328011925/VeraCrypt_1.21_Source.tar.bz2
tar -xvf VeraCrypt_1.21_Source.tar.bz2
cd src
make -j$(nproc)
```
