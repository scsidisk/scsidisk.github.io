# HOWTO: Using FileMerge (opendiff) with Git on OSX

FileMerge (opendiff) can really come in handy when you need to visually compare
merging conflicts. Other times it's just a nice visual way to review your days
work.

The following method works by creating a simple bash script (git-diff-cmd.sh)
that sets us up with the proper command line arguments for Git to pass off
files to FileMerge.

1. Open the bash script for editing:

        vi ~/bin/git-diff-cmd.sh

2. Paste the following code:

        #!/bin/sh
        /usr/bin/opendiff "$2" "$5" -merge "$1"

3. Make the bash script executable:

        chmod +x ~/bin/git-diff-cmd.sh

4. Tell Git (globally) to run our bash script when 'git diff' is issued:

        git config --global diff.external ~/bin/git-diff-cmd.sh

5. Tell Git (globally) to opendiff during a 'git mergetool'

        git config --global merge.tool opendiff


Now head over to your Git-aware project directory and issue a
`git diff /path/to/modified/file.py` and FileMerge will pop up showing you the
differences against it and HEAD.

You can also do things like `git diff --cached` to review all the changes
against HEAD.

If you get an error like:

        xcode-select: Error: No Xcode folder is set. Run xcode-select -switch <xcode_folder_path>
        to set the path to the Xcode folder. Error: /usr/bin/xcode-select returned unexpected error.

Then run:

        sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer/

For reference: http://stackoverflow.com/questions/9300748/opendiff-does-not-start-anymore


 I wanted to use a diff tool for Git on my MacBook Pro. I found that opendiff can be used as a diff tool. But, you need to configure git to use this tool.

git config --global diff.tool opendiff
git config --global merge.tool opendiff
git config --global difftool.prompt false