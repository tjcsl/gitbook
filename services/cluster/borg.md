# Borg

Borg is experimental, so documentation is limited for now.

Pertinent things to know:
* We have 40 (`borgw2[01-40]`), but can only run 8 (`borgw20[1-8]`)
* They are named after constellations
* They run the same OS (centos (theo pls link)) and ansible play as the rest of the cluster.
* To use them from infosphere, add the `-p compute2` flag to your `srun` or `salloc` play.
