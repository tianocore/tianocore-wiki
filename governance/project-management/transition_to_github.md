# Transition To Github

The transition to GitHub will start February 2nd at around 10AM PST
(UTC-8). We estimate it to be completed by February 4th at 5PM PST.

Here are the planned transition tasks along with their status:

- ✅ change commit emails from svn to github
- ✅ update web pages regarding repo url =>
  [edk2](https://github.com/tianocore/edk2)
- ✅ email notification of sourceforge svn becoming read-only
- ✅ disable write access on sourceforge
- ✅ sync svn => git one last time
- ✅ backup sourceforge EDK II svn database
- ✅ test EDK II svn backup
- ✅ backup sourceforge EDK II FAT svn database
- ✅ test EDK II FAT svn backup
- ✅ disable svn to github mirroring
- ✅ enable push access for package maintainers on github
- ✅ push test commit to master modifying Maintainers.txt
- ✅ email notification of github push access

Post transition follow on tasks:

- ✅ Automated mirroring of git repo to git mirrors
- ✅ Automated mirroring of git repo (master, UDK branches) back to deprecated
  svn repo
- ✅ Archive old svn branches to separate [git repo](https://github.com/tianocore/edk2-archive)
