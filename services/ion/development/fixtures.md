# Fixtures

Fixtures are SQL queries that use a real production data set to populate the development database.  Since this contains personal information, access should be restricted.  Fixtures may be obtained \(through a secure method\) from an Ion admin if you are developing for Ion.

## Import

Once you get a zipped fixtures file, unzip the file to a `fixtures` directory in your main work directory.  Within the main work directory, run `./scripts/import_fixtures.sh` to import the fixtures.  This process may take some time.

## Export

To export fixtures from production, you should SSH to root on `ion`.  Navigate to the main directory, make a directory called `fixtures`, and run `./scripts/export_fixtures.sh`.  Your fixtures should be populated in the `fixtures` directory and can now be zipped for transfer.

