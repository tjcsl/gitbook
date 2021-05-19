# Data Generation

## Generating Users and Eighth Periods

Once you have your Vagrant environment set up, log into [GitLab](https://gitlab.tjhsst.edu/). If you do not have access to GitLab, email sysadmins@tjhsst to request access.

After you log in, clone the Ion Fake Data repo [here](https://gitlab.tjhsst.edu/sysadmins/web/ion/ion-fake-data). Follow the instructions in `README.md` to generate json files for importing user and eighth period activity data.

Move the json files into the main directory of your local intranet copy, and ssh into your Vagrant machine. Then run `python manage.py import_users user_import.json` and `python manage.py import_eighth eighth_import.json` to import user and eighth period activities.

In order to show eighth period activities on Ion, you need to create eighth period blocks. Run `python manage.py dev_create_blocks mm/dd/yyyy` and fill in an end date. This script will create blocks every Wednesday and Friday from the current date until the end date.

Once blocks have been created, you can generate signups for the blocks on a specific date by running `python manage.py dev_generate_signups mm/dd/yyyy` by inputting a specific date which has eighth period blocks. The commands to generate blocks and signups for testing should ONLY be used in your local version of Ion.

Now you have enough data to begin testing changes on users and eighth periods.

