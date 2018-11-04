# OpenAFS/Cross-cell

**OpenAFS Cross-cell** authentication is currently used so all students can access portions of AFS file space (for temporarily relocated netware files).

The process by which this occurs is mostly automatic, but the local cell giving access (CSL.TJHSST.EDU in our case), must create the group system:authuser@foreign.cell where foreign.cell is the foreign cell; in our case, LOCAL.TJHSST.EDU. So, first, the group system:authuser@local.tjhsst.edu must be created.

After that is done, anyone that tries to `aklog` with tickets from the LOCAL realm will automatically create themselves a foreign account in the CSL cell, if they don't already have one. Keep in mind that the number of foreign users for the LOCAL realm/cell is capped by the group quota of system:authuser@local.tjhsst.edu. Originally, this was set to 0, which actually means a limit of 30 (confusingly enough). As of the time of this writing, it is set to 1800 to accomodate for all students, but set it to be larger with `pts setfields` if it needs to be larger. If you attempt to exceed the limit, pts will issue an error saying you cannot create any more groups. 
