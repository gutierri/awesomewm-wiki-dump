---
title: Awesome-3-git-debian
permalink: /Awesome-3-git-debian/
---

The debian builder script is optimized for building GIT version of Awesome.

To setup a building environment you should:

-   clone official repository

` git clone `[`git://git.naquadah.org/awesome.git`](git://git.naquadah.org/awesome.git)

-   Change to fetched repository

` cd awesome`

-   Add debian source

` git remote add origin-debian `[`git://git.debian.org/git/users/acid/awesome.git`](git://git.debian.org/git/users/acid/awesome.git)

-   Fetch objects from debian

` git fetch origin-debian`

-   build binaries

` cmake -DCMAKE_PREFIX_PATH=/usr -DSYSCONFDIR=/etc && make`

-   install awesome

` sudo make install`
` sudo ldconfig -v`

-   [update the configuration file](/Awesome_3.4_to_3.5 "wikilink") or use the new version

` sudo cp awesomerc.lua /etc/xdg/awesome/rc.lua`

` [user]`
`         email = your-email@ddr.ess`
`         name = Your name`
` [core]`
`         repositoryformatversion = 0`
`         filemode = true`
`         bare = false`
`         logallrefupdates = true`
` [remote "origin"]`
`         url = `[`git://git.naquadah.org/awesome.git`](git://git.naquadah.org/awesome.git)
`         fetch = +refs/heads/*:refs/remotes/origin/*`
` [branch "master"]`
`         remote = origin`
`         merge = refs/heads/master`
` [remote "origin-debian"]`
`         url = `[`git://git.debian.org/git/users/acid/awesome.git`](git://git.debian.org/git/users/acid/awesome.git)
`         fetch = +refs/heads/*:refs/remotes/origin-debian/*`

[Category:Awesome3](/Category:Awesome3 "wikilink")