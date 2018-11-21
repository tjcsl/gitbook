---
description: How to run tjSTAR signups and tech support
---

# tjSTAR

The CSL is responsible for running signups and tech support for the annual tjSTAR, which usually happens in late May or early June.

> The name of the conference should be tjSTAR, not tjStar \(that just doesn't make sense -- STAR is short for _Symposium to Advance Research_.

### Separation of Duties

#### tjSTAR Committee

* Put together master schedule
* Compile Constraints and Info for:
  * Professional Presenters
  * CHUM Presentations
  * Foreign Language Presentations
  * Keynote Speaker
  * tl;dr: everyone who is not IBET or Senior Research Lab \(SRL\)

#### Lab Directors / IBET Teachers

* Compile constraints for seniors / freshmen
* Choose _who_ goes _when_, and indicate preferences \(or lack thereof\) for rooms.
  * This includes which projects are presented together, etc.

#### Sysadmins

* Should ideally be a black box:
  * tjSTAR committee sends the **3** spreadsheets, signups site pops out, populated with all the activities.

### Timeline

#### Beginning of the School Year

* Send out email detailing how data entry is supposed to work.
* Hold meetings with lab directors to explain what they need to have and the deadline.
* Hold meetings with IBET teachers to explain what they need to have and the deadline
* Deadline should be 2 weeks before registration opens

#### 3 weeks before registration opens

* Send out reminder email to teachers with deadline
* Meet with tjSTAR committee to make sure they know what they have to do

#### 2 weeks before registration opens

* Clear up any confusions / issues with scheduling
* Load database of activities
* Meet with lab directors / teachers if necessary

#### Registration opens

* See how nothing is scheduled to happen within 2 weeks of registration? Be firm on the 2 week deadline.

#### Registration closes

#### 1 day before tjSTAR

* Send confirmation emails to students

### Developing tjSTAR Signup Site

To set up a development environment:

* clone the site
* copy `settings_secret.py.sample` to `settings_secret.py`
* go to ion.tjhsst.edu/oauth:
  * add a new project
  * select client type "confidential"
  * select grant type "authorization-code"
  * add `http://localhost:8000/complete/ion/` as a redirect uri
* in the `settings_secret.py`:
  * set a `SECRET_KEY`
  * add the social auth key and secret \(the client ID and secret\)
  * comment out the databases section: we will stick with sqlite
  * add your name and TJ email as the admin.
  * comment out everything underneath `# Production`
* in the `settings.py`:
  * set the `SENIOR_YEAR`
  * set the `TJSTAR_DATE`

### Guidelines for Data Entry

> Should be emailed at the beginning of the year, then again closer to the data submission deadline
>
> You should attach the files from the previous year as an example.

**Activities:** The `activities.xlsx` file contains the template for sessions and activities.

The excel file should have multiple sheets, with each sheet representing a session. The script will ignore any sheets where the sheet name does not start with the word "Session".

Each sheet should have the following columns, in order:

* Session \(ex: A, B, C\)
* Category
* Title
* Description
* Student Speaker
* Outside Speaker
* Location
* Faculty Monitor\(s\)
* Capacity

**Constraints \(Not Seniors\):** The `constraints.xlsx` file contains the template for non-senior constraints. I have included one example row for each sheet, in gray text. This should \(obviously\) be deleted and replaced.

The name of the sheet should be the activity that the user should be signed up for. All rows will be ignored until the first column is "Name\(s\)". After this row has been reached, subsequent rows will be processed. The constraint sheets should have the following columns:

* Name\(s\)
* Student ID\(s\)
* Sessions Required
* Room
* Again, consult the spreadsheet template if this description is confusing.

**Constraints \(Seniors\)** Seniors have a separate constraints file: `seniorconstraints.xlsx`. I've attached last year's constraints file for reference.

The format for senior constraints is much more lenient. Here are the minimum requirements:

* The name of every sheet is the name of the lab
  * ex: "Astronomy & Astrophysics" or "JUMP Lab"
  * Exactly one column in every row contains the Block ID. This should be the same column for every row in that sheet.
* the Block ID is an uppercase letter with an optional dash and number.
  * ex: "A" and "A-2" are both valid Block IDs
* Exactly one column in every row contains the room number. This should be the same column for every row in that sheet.
  * the room number is a number \(max 3 digits long\) with an optional uppercase letter at the end
  * ex: "200" and "200C" are both valid. "200c" and "cafeteria" are invalid
* A variable number of columns in every row contain the student ID of the student presenter. The student ID should be the only thing in the cell.
  * the student ID should be 5-13 digits long

Note that the lab name, lab director and student names, email addresses, presentation name, and comments are not required, and will be ignored.

While these other fields are allowed, they must not look like a Block ID, room number, or student ID. Otherwise, errors may occur. For instance, if a student entered "A-2" as his name \(for whatever reason\), the script will try to parse that column as a Block ID. Very bad!

### Setup for Production

1. `git pull` and make sure the codebase is up to date.
2. Rename the previous year's database to `db.sqlite3.<year>`
3. Restart the app
   * This will generate a new `db.sqlite3` databse
4. The permissions will likely be incorrect.
   * `ksu` and change the permissions to be `644`
   * If you don't have `ksu`, ask another sysadmin who does to change the permissions
5. Run `./manage.py migrate` to set up the database.

### Running tjSTAR Signups

Make morning announcements and Ion announcement reminding people to sign up.

#### Ion Widget

On production tjSTAR, run `./manage.py generate_widget_uuids`.

This will output a `uuids.json` file, which you should then `scp` to the Ion VM. If you do not have access to the Ion VM, ask a sysadmin who does to copy it for you and proceed with the following steps.

On the Ion VM, open a Django shell and execute the following:

```text
import json
uuids = json.load(open('./uuids.json', 'r'))
TJStarUUIDMap.objects.all().delete()
for user in uuids:
    print(user)
    try:
        u = User.objects.get(username=user)
        t = TJStarUUIDMap(user=u, uuids[user])
        t.save()
    except:
        print('\tFailed')
```

Then open `intranet/settings/secret.py` and change `TJSTAR_MAP` to `True`.

Restart Ion.

#### Schedule Change Requests

_Schedule change requests should only come from faculty or members of the tjSTAR committee._

The types of change requests are as follows, ranked from least destructive to most destructive.

1. **Room Change**
   * Easiest change
   * Go into Django Admin, find the activity, and change the room.
   * Run the room validation script to make sure no room is overbooked.
2. **Signup Change**
   * Have to add a new signup, delete preexisting signup if it exists.
   * Run signup validation to make sure there are no duplicate signups
3. **Activity Change**
   * If the activity needs to be rescheduled
   * Create new activity
   * Delete old activity
   * Email people who were signed up for the old activity
   * Run room validation 

**IMPORTANT:** Activity changes _remove_ signups. Make sure people who were signed up know to sign up for a different activity in the affected blocks.

#### After Signups Close:

**1. Make sure there are faculty monitors in all the rooms. You can check for rooms without faculty monitors with:**

```text
for a in Activity.objects.filter(sponsors__isnull=True).order_by('name','block__name'):
    print(str(a))
```

It's generally ok for these to not have sponsors:

* _tjStar Volunteer Team_ \(as long as steering committee does\)
* _tjStar Tech Team_
* _Lunch_
* _Keynote_ \(See below\)
* _Introductory Remarks_

**2. Assign teachers who should be in auditorium for keynote \(this will come from committee or Ms. Galanos or something\)**

```text
# `teachers` is a comma-delimited string of last names:
# ex: Razzino,Seyler,Bell,Burnett,Miller,McAleer,Muir,JirariScavotto,Wu

keynote = Activity.objects.get(name="Keynote Speaker -- Juniors and Seniors")
teachers = "Razzino,Seyler,Bell,Burnett,Miller,McAleer,Muir,JirariScavotto,Wu"

for teacher in teachers.split(','):
    t = Teacher.objects.get(user__last_name=teacher)
    keynote.sponsors.add(t)

keynote.save()
```

**3. Split the underclassmen into actual classrooms for the Keynote.**

This involves a bit of code \(and "manual" data entry\)

These variables need to be loaded:

```text
# note: int vs str type doesn't matter
# these are the faculty monitors for the keynote rooms
teachers = {
    "Dell": "235",
    "Scholla": "236",
    "Sheptyck": "253",
    # and so on
} 

ALTERNATIVE_SPELLINGS = {
    # copy dict from import_room_monitors.py 
}
```

This part can be basically copy/pasted:

```text
blk = Block.objects.get(name="Keynote")
cat = Category.objects.get(name="Administrative")
for t in teachers:
    room_num = teachers[t]
    print(t, room_num)
    room, _ = Room.objects.get_or_create(name=room_num)
    t = ALTERNATE_SPELLINGS[t] if t in ALTERNATE_SPELLINGS else t
    last = t.split()[0].replace(';',' ')
    if len(t.split()) > 1:
        first = ' '.join(t.split()[1:])
    teacher = Teacher.objects.filter(user__last_name=last)
    if teacher.count() > 1:
        teacher = teacher.filter(user__first_name=first).first()
    else:
        teacher = teacher.first()
    a, _ = Activity.objects.get_or_create(name="Keynote Speaker -- Freshmen and Sophomores", room=room, capacity=40, block=blk, category=cat)
    a.sponsors.add(teacher)
    a.save()
old_keynote = Activity.objects.get(name="Keynote Speaker -- Freshmen and Sophomores", room__name="Homeroom")
new_keynotes = Activity.objects.filter(name="Keynote Speaker -- Freshmen and Sophomores").exclude(room__name="Homeroom").distinct()
signups = Signup.objects.filter(activity=old_keynote)
keynote_count = new_keynotes.count()
for i in range(len(signups)):
    s = signups[i]
    print("Signing up {}".format(s.student))
    s.activity = new_keynotes[i % keynote_count]
    s.save()
```

### Special Notes 2018

#### Faculty Room Monitors

Faculty monitors were submitted late. This is not ideal! The `import_faculty_excel` imports an excel file that's formatted like `faculty_monitors.xlsx` on Director. In the future, hopefully, faculty monitors will be assigned in advance, but if not, that is the format to use in order to take advantage of preexisting code.

**IMPORTANT:** The sheets must be name "Block A", "Block B", etc. This is **different** from the master schedule, where they must be named "Session A", "Session B", etc.

#### SOL Testing

SOL rooms cannot be used. Ideally, acquire a list of SOL rooms prior to inputting data.

