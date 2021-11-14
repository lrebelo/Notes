# Notes on ZFS on TrueNAS

## Renaming a ZFS pool
TrueNAS does not have (as of writting this) a way to rename a pool via th UI so!

Steps:
* Export the pool via the UI (without destrying the data...)
* Open a shell
* `zpool import oldpoolname newpoolname`
* `zpool export newpoolname`
* Go over to the UI and Import the pool!

Easy peasy lemon squeezy! :D
