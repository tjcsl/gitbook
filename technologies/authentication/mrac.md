# MRAC

This is a page for the new MRAC system, which as of April 2015 has been installed in the new Machine Room 200D. The Raspberry Pi is accessible using the hostname "auth-rasppi" and the root password is in the [CSL Passcard](passcard.md).

#### Construction

The MRAC system was purchased as a replacement for the old and outdated RFID access system in place in the old computer systems lab. The new system consists of an ACR122U USB NFC reader, a Raspberry Pi, an electronic strike \(model\#?\) and a custom power circuit board. The Raspberry Pi runs a custom made control program \(available on [GitLab](https://gitlab.tjhsst.edu/sysadmins/auth-rasppi/blob/master/auth.c) for people with access\).

The custom power circuit board uses an external power supply to boost the output from the Raspberry Pi's 3.3V GPIO pins to the 12V required by the electronic strike.

The cards used with the system are Mifare Classic 1k cards.

Here is a schematic:

[![MRAC Schematic.png](https://livedoc.tjhsst.edu/images/6/64/MRAC_Schematic.png)](https://livedoc.tjhsst.edu/wiki/File:MRAC_Schematic.png)

#### Key Format

Mifare Classic keys consist of 16 sectors, each with three 16 byte data sections and one 16 byte authentication header. The MRAC system changes the keys located in the authentication header to CSL-created keys in the first sector, then uses two 16 byte data sections to store a card-specific 32 byte key.

The 32 byte key is read when the card is scanned, and if the key is among an automatically generated list of allowed keys, the user is allowed access to the machine room.

The list of allowed keys is stored in a file on the raspberry pi, with one key \(32 hex bytes represented in ASCII, so 64 characters\) per line. The list of allowed keys is controlled according to the Programming section below.

#### Programming

The current system mainly uses a Master/Action key system to control the list of valid keys. The keys should be kept in a secure location, preferably inside the machine room. To perform an administrative action, scan the master and/or action keys in the correct order for the specific action, removing each key before scanning the next. The control light should turn orange during the programming sequence, then briefly flash green when the action is fully completed. Below are sequences for various actions currently programmed \(M means Master, A means Action\).

|  Sequence |  Action Name |  Description |
| :--- | :--- | :--- |
|  MMA &lt;new card&gt; |  Add Key |  Will generate a key, write it to &lt;new card&gt;, and add it to the list of allowed keys. |
|  MAA &lt;card&gt; |  Wipe Key |  Wipe an existing key from &lt;card&gt;. Does not check / remove the key from the list of allowed keys. For this, use Delete Key. |
|  MMM &lt;card&gt; |  Delete Key |  Remove the key on &lt;card&gt; from the list of allowed keys. |
|  MAM &lt;card&gt; |  Add Key, No Regenerate |  Add a key already existing on &lt;card&gt; to the list of allowed keys. Will not generate a new key and write it. |
|  AMA |  Reload Database |  Reload the key file from disk. Useful if it has been manually edited. |

#### Administration

To add a card, take a blank or old unused card, and issue the Add Key command using the Master and Action keys. **Print and put on the card a label with the ID of the user who the card is being issued to \(such as 2017sdamashe\).** In addition, **connect to the Raspberry Pi and edit the key file - after the key of the new card, add the user ID on the same line** \(the line should look like "&lt;64 characters&gt; 2017sdamashe" for me\). This is for key accountability; each access is logged to the system.

#### Issuing Access

Access to the Machine Room may be issued to authorized users that meet one of these requirements:

1. User is a student systems administrator of the TJHSST Computer Systems Lab.
2. User has full root access on a production system in the Machine Room.
3. User is a teacher or staff member of TJHSST who originally has access to the Machine Room through a normal key. Note that a normal key will still function in the Machine Room door lock.

#### Use of Access

An authorized user may:

* Enter the Machine Room.
* Allow a non-authorized individual\(s\) to accompany the authorized user into the Machine Room only if the individual\(s\) needs to interact with systems in the Machine Room, and only with permission of a current student systems administrator. This non-authorized individual must be escorted by the authorized user \(i.e. they cannot be left alone in the Machine Room\).

Outside visitors touring the school are allowed to tour the Machine Room. A student systems administrator or TJHSST staff member must be present to supervise the visitor's actions. The visitor should NOT receive an access card for the duration of their visit.

Contractors and other maintenance individuals that require access to the Machine Room for a length of time greater than one day may be issued an access card for the duration of their visit. They must return the access card to the CSL immediately after completion of predetermined task.

#### Revoking Access

Access to the Machine Room may be revoked if:

* User gives/shares an access card to/with other non-authorized individual \(this does not include allowing non-authorized individual\(s\) access to the machine room for temporary reasons\).
* User violates the TJHSST Computer Systems Lab Policy, TJHSST Network User Guidelines, or FCPS Acceptable Use Policy.
* User graduates from/leaves \(students/staff\) TJHSST. This is usually June of the graduation year for students.

#### The Policy

This policy may be modified for certain special cases at any time. The CSL director or teacher has the final word on any conflict.

