# Transition To Github

The transition to GitHub will start February 2nd at around 10AM PST
(UTC-8). We estimate it to be completed by February 4th at 5PM PST.

Here are the planned transition tasks along with their status:

* [x] change commit emails from svn to github
* [x] update web pages regarding repo url =>
      [edk2](https://github.com/tianocore/edk2)
* [x] email notification of sourceforge svn becoming read-only
* [x] disable write access on sourceforge
* [x] sync svn => git one last time
* [x] backup sourceforge EDK II svn database
* [x] test EDK II svn backup
* [x] backup sourceforge EDK II FAT svn database
* [x] test EDK II FAT svn backup
* [x] disable svn to github mirroring
* [x] enable push access for package maintainers on github
* [x] push test commit to master modifying Maintainers.txt
* [x] email notification of github push access

Post transition follow on tasks:

* [x] Automated mirroring of git repo to git mirrors
* [x] Automated mirroring of git repo (master, UDK branches) back to deprecated
      svn repo
* [x] Archive old svn branches to separate [git repo](https://github.com/tianocore/edk2-archive)
