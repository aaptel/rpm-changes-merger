# git merge driver for RPM changes log files

This script is able to do 3-way merges for .changes file
automatically.


Given this scenario:


    branch A (added entries)                   branch B (added entries)
             |                                          |
             |                                          |
             |                                          |
             |                                          |
             +--------------  base commit --------------+
                               on master


When merging B and A into master git usually fails with a merge
conflicts.  In the cases where both branches just add entries it is
pretty easy to solve: add both entries ordered by their timestamp.

If any branches removes or edit previous entries the script will also
fail and let you sort it out.

Run the script with -h or --help for more.

# Installation

Simply copy the script or symlink it to a directory in your $PATH or
use full paths to the script in the configuration.

## Global

    git config --global merge.rpm-changes-merger.name   "driver for rpm changes log files"
    git config --global merge.rpm-changes-merger.driver "$HOME/bin/rpm-changes-merger -o %A %O %A %B'

If you don't already have a global user attributes file:

    git config --global core.attributesfile ~/.gitattributes

Add this to your attributes file:

    *.changes merge=rpm-changes-merger

## Single repo

    git config --local merge.rpm-changes-merger.name   "driver for rpm changes log files"
    git config --local merge.rpm-changes-merger.driver "$HOME/bin/rpm-changes-merger -o %A %O %A %B
    echo '*.changes merge=rpm-changes-merger' >> .git/info/attributes
