# Production

Steps to install Ion are described in our Ansible playbooks.

Information on how to run Ion in production is located in [our runbooks](../../general/documentation/runbooks.md).

## Troubleshooting tips

1. A 502 Bad Gateway ("Unable to contact an application server") indicates a problem with Daphne.
2. If you can access static files (like [https://ion.tjhsst.edu/static/css/base.css](https://ion.tjhsst.edu/static/css/base.css)) with no problem, but dynamic pages like the login page give errors or are very slow to load, it's almost definitely a problem with either Daphne or the database. (Conversely, if static files exhibit the same problem, it's probably an Nginx issue.)
   1. You might be able to resolve Daphne issues by increasing the number of workers. Just make sure that 1) you add them to the Nginx config and 2) you increase the PostgreSQL connection limit appropriately (you'll want to run `systemctl restart postgresql && supervisorctl reread && supervisorctl update && systemctl restart nginx`when you've edited all the config files).\
      Each Daphne worker seems to require about 75 connections, and you'll want a buffer over that.
3. If Ion is experiencing performance issues, try these troubleshooting steps:
   1. Try and access a static file (see #2 above).
   2. SSH to Ion and run `iotop`. If you see high disk usage, database access might be the slowdown.
