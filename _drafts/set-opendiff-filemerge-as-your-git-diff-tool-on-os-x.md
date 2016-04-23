Set opendiff (FileMerge) as your git diff tool on OS X
=======================================================

Thomas
2013

Are you unhappy with the command line diff tools provided by git? If so, this tutorial is for you.

FileMerge is a utility that comes with OS X / Xcode, although it is hidden and not the easiest to get to. You can dig around the Xcode.app directory until you find it, although the command 'opendiff' should already exist in your terminal. To see a visual diff of two files, run ‘opendiff file1 file2’ in your terminal, and a GUI app will appear.

To set FileMerge as your git diff tool, first create a simple script that will be the translator between git and the FileMerge utility. I keep mine in the following location:

    vim /usr/local/bin/gitdiff.sh

The contents of the file will look like this:

    #!/bin/sh
    /usr/bin/opendiff "$2" "$5" -merge "$1"

Once the script has been made, you’ll want it to be executable

    chmod u+x /usr/local/bin/gitdiff.sh

Finally, tell git that you want to set it up as your default merge tool:

    git config --global diff.external /usr/local/bin/gitdiff.sh

If you later decide you hate it, run this command to go back:

    git config --global --unset diff.external