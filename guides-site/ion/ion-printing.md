---
description: Using the Printing service on Ion
---

# Ion Printing

## Printing

The Printing feature on Ion is a service that the CSL provides to students and staff member to be able to print to the 3 Syslab printers and to the 9 commons printers around the school. It's a service made available to students who don't use/have a school-provided laptop, use one of our 60+ workstations in the CSL, or needs to print something on-the-go.

The contact person for the printing service and associated infrastructure is [the Printing Lead](broken-reference).

### Printers

We currently have 12 functioning printers around the school. Three of them are located in the Computer Systems Lab, with one in each room:

#### In the Syslab

* Room 202 Printer:
  * Model: HP LaserJet P3015
* Room 200c Printer:
  * Model: HP LaserJet P3015
* Room 200 Printer:
  * Model: HP LaserJet P3015

#### Around the School:

These printers were add to Ion in late 2023 as the demand for use of Ion printing has increased.

* Carver Commons Printer:
  * Model: HP LaserJet 4100
* Curie Commons Printer:
  * Model: HP LaserJet 4100
* Einstein Commons Printer:
  * Model: HP LaserJet 4100
* Faraday Commons Printer:
  * Model: HP LaserJet 4100
* Gandhi Commons Printer:
  * Model: HP LaserJet 4100
* Hopper Commons Printer:
  * Model: HP LaserJet 4100
* Newton Commons Printer:
  * Model: HP LaserJet 4100
* Tesla Commons Printer:
  * Model: HP LaserJet 4100
* Turing Commons Printer:
  * Model: HP LaserJet 4100

### Ion Printing

To Print from Ion:

1. Access the web interface at [ion.tjhsst.edu/printing](https://ion.tjhsst.edu/printing). You need to be on school wifi (`Fairfax`) to access the service.
2. Select the file you want to print. Please make sure that the file type is supported as well. Here are the ones that currently are supported by Ion:
   1. **.pdf**: Adobe PDF
   2. **.ps**: Adobe PostScript
   3. **.odt**: OpenDocument Text Document
   4. **.txt**: Plain Text
   5. **.doc** and **.docx** (not recommended; format may come as off. Would recommend converting it to a PDF): Word Document
3. Select the printer you want to print to. Choosing the one close by you is highly recommended.
4. Check to see how many pages you are printing. The maximum amount of pages you can print at once is 16.
5. Check if you want your documents double-sided and/or fit-to-page.
6. Print.

## Workstation Printing

Workstation Printing can be done by going to Ion, or printing directly from the command line. To Print from the command line:

1. Access the terminal.
2. Change directory to the file you want to print (not required but recommended).
3. **Recommended:** Check which printers are online and not disabled. You can use the command `lpstat -p` to see a list of printers and their status:

<pre><code><strong>printer Carver now printing Carver-26732.  enabled since Fri 05 Jan 2024 12:43:52 PM EST
</strong>        Connected to printer.
printer Curie now printing Curie-26437.  enabled since Fri 05 Jan 2024 12:41:44 PM EST
        Connected to printer.
printer Einstein now printing Einstein-26703.  enabled since Fri 05 Jan 2024 12:17:40 PM EST
        Connected to printer.
printer Faraday now printing Faraday-26555.  enabled since Fri 05 Jan 2024 12:56:01 PM EST
        Connected to printer.
printer Gandhi is idle.  enabled since Fri 05 Jan 2024 03:05:52 AM EST
printer Hopper now printing Hopper-26618.  enabled since Fri 05 Jan 2024 11:44:17 AM EST
        Connected to printer.
printer Newton now printing Newton-26468.  enabled since Fri 05 Jan 2024 12:05:57 PM EST
        The printer is not responding.
printer Room_200C disabled since Wed 03 Jan 2024 02:49:44 PM EST -
        Paused
printer Room_202 disabled since Tue 28 Nov 2023 11:02:06 PM EST -
        Paused
printer Tesla is idle.  enabled since Fri 05 Jan 2024 12:28:41 PM EST
printer Turing now printing Turing-26572.  enabled since Fri 05 Jan 2024 11:45:57 AM EST
        Connected to printer.
</code></pre>

4. To print to a specific printer, you can use this command `lp -d <PRINTER> <FILENAME>`, where you replace the `<PRINTER>` with the printer of your choice, and the `<FILENAME>` to file you want to print.

## Abuse Protection

Jobs are currently limited to 16 pages. Abuse of printing privileges may result in appropriate punishment as determined by the Faculty Sponsor and lead Sysadmins.

## Best Practices

* Check to see if the printer contains ink or paper, if one of the two are low or missing, please contact a Sysadmin.
* If you sent to the printer a print job (you have hit the 'Print' button on Ion) and the printer seems not to work, please check the following:
  * The tray: This is where paper is stored. For the printers in the Syslab, pull the tray located at the bottom of the printer. **DO NOT OPEN THE TRAY ON THE SIDE OF THE PRINTER!** For the printers located in commons, pull the bottom tray. If there is paper there, ask a Sysadmin about the printer. Do not send another print job.
  * Disablement/Maintenance: There are in some cases where a printer may be disabled or temporarily down for maintenance and with that case can only be changed by a Printing Lead or current Lead Sysadmins.
  * Jammed: If a printer is jammed, do not attempt to unjam the printer! This will most likely cause the printer to be damaged. Instead, ask a Sysadmin or a nearby teacher to assist.
* Highly Recommend to be present at the printer when printing.
  * If in some circumstances where you printed from a different room/common than the printer is in, please pick up your documents. If this is not done, you may be subjected to a possible removal of printing privileges.
* Please avoid printing multiple times unless you have picked up your first copies.
