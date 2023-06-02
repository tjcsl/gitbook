# MRAC

The **Machine Room Access Control** (MRAC) is a system used for controlling access to the Machine Room (Room 200D).  It uses NFC cards and a reader to authenticate authorized personnel into the Machine Room.

## General

This is a page for the new MRAC system, which as of April 2015 has been installed in the new Machine Room 200D. The Raspberry Pi is accessible using the hostname "auth-rasppi" and the root password is in the [CSL Passcard](passcard/).

## Construction

The MRAC system was purchased as a replacement for the old and outdated RFID access system in place in the old computer systems lab. The new system consists of an ACR122U USB NFC reader, a Raspberry Pi, an electronic strike (model#?) and a custom power circuit board. The Raspberry Pi runs a custom made control program (available on [GitLab](https://gitlab.tjhsst.edu/sysadmins/auth-rasppi/blob/master/auth.c) for people with access).

The custom power circuit board uses an external power supply to boost the output from the Raspberry Pi's 3.3V GPIO pins to the 12V required by the electronic strike.

The cards used with the system are Mifare Classic 1k cards.

Here is a schematic:

![](../../.gitbook/assets/mrac\_schematic.png)

## Key Format

Mifare Classic keys consist of 16 sectors, each with three 16 byte data sections and one 16 byte authentication header. The MRAC system changes the keys located in the authentication header to CSL-created keys in the first sector, then uses two 16 byte data sections to store a card-specific 32 byte key.

The 32 byte key is read when the card is scanned, and if the key is among an automatically generated list of allowed keys, the user is allowed access to the machine room.

The list of allowed keys is stored in a file on the raspberry pi, with one key (32 hex bytes represented in ASCII, so 64 characters) per line. The list of allowed keys is controlled according to the Programming section below.

## Programming

The current system mainly uses a Master/Action key system to control the list of valid keys. The keys should be kept in a secure location, preferably inside the machine room. To perform an administrative action, scan the master and/or action keys in the correct order for the specific action, removing each key before scanning the next. The control light should turn orange during the programming sequence, then briefly flash green when the action is fully completed. Below are sequences for various actions currently programmed (M means Master, A means Action).

|  Sequence        |  Action Name            |  Description                                                                                                                 |
| ---------------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
|  MMA \<new card> |  Add Key                |  Will generate a key, write it to \<new card>, and add it to the list of allowed keys.                                       |
|  MAA \<card>     |  Wipe Key               |  Wipe an existing key from \<card>. Does not check / remove the key from the list of allowed keys. For this, use Delete Key. |
|  MMM \<card>     |  Delete Key             |  Remove the key on \<card> from the list of allowed keys.                                                                    |
|  MAM \<card>     |  Add Key, No Regenerate |  Add a key already existing on \<card> to the list of allowed keys. Will not generate a new key and write it.                |
|  AMA             |  Reload Database        |  Reload the key file from disk. Useful if it has been manually edited.                                                       |

## Administration

To add a card, take a blank or old unused card, and issue the Add Key command using the Master and Action keys. **Print and put on the card a label with the ID of the user who the card is being issued to (such as 2017sdamashe).** In addition, **connect to the Raspberry Pi and edit the key file - after the key of the new card, add the user ID on the same line** (the line should look like "<64 characters> 2017sdamashe" for me). This is for key accountability; each access is logged to the system.

This policy may be modified for certain special cases at any time. The CSL director or teacher has the final word on any conflict.
