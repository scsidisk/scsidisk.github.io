Using DiffMerge as your Git visual merge and diff tool

Posted by Todd Huss on Aug 17, 2011 in Software Development, code, Git, Mac OS X    | 36 Comments
Install DiffMerge for Git on Mac OS XOur favorite (and free) visual diff and merge tool for OS X (as well as Linux and Windows) is DiffMerge. It makes resolving nasty Git branch conflicts a snap (relatively speaking). Here’s how to install it and configure it with Git on OS X:

Download the DiffMerge OS X installer. Be sure to download the Installer version. There’s also a DMG version but then you’ll have to manually install the diffmerge command line script.

Once you’ve downloaded it go into terminal and make sure you can run it from the command line by typing diffmerge and hitting return.

To configure Git to use DiffMerge run these commands from the command line:

    git config --global diff.tool diffmerge
    git config --global difftool.diffmerge.cmd 'diffmerge "$LOCAL" "$REMOTE"'
    git config --global merge.tool diffmerge
    git config --global mergetool.diffmerge.cmd 'diffmerge --merge --result="$MERGED" "$LOCAL" "$(if test -f "$BASE"; then echo "$BASE"; else echo "$LOCAL"; fi)" "$REMOTE"'
    git config --global mergetool.diffmerge.trustExitCode true

Now, whenever you want it to launch diffs just use difftool where you’d normally use diff:

    # diff the local file.m against the checked-in version
    git difftool file.m

    # diff the local file.m against the version in some-feature-branch
    git difftool some-feature-branch file.m

    # diff the file.m from the Build-54 tag to the Build-55 tag
    git difftool Build-54..Build-55 file.m

To resolve merge conflicts, just run git mergetool:

    git mergetool