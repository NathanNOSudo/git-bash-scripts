
<h1 style="text-align:''center;">Handy Bash scripts for Git. </h1>
Bash scripts can make your life easier when you're working with Git repositories. Your probably thinking there's no need; everything I need to do can be done with Git commands...and your correct.
The fact of the matter is it's incredibly more convienent to use the scripts than trying to figure out the appropriate Git command to do what I want. Scripts make life easier; work smarter, not harder.

## Scripts:

* [gitlog](#gitlog)
* [gitlog.id](#gitlog.id)
* [gitlog.id2](#gitlog.id2)
* [gitlog.grep](#gitlog.grep)
* [coming-soon](#coming-soon)
* [coming-soon](#coming-soon)
* [fetchall](#fetchall)
* [updateall](#updateall)



<br></br>
**<h2 style="text-align: center;">gitlog</h2>**

<p style="text-align: center;">gitlog prints an abbreviated list of current patches against the master version. It prints them from oldest to newest and shows the author and description, with H for HEAD, ^ for HEAD^, 2 for HEAD~2, and so forth.
<br></br>
To see what patches are on a different branch, just specify an alternate branch:

```sh
$ gitlog prototype7
```
<br></br>
**<h2 style="text-align: center;">gitlog.id</h2>**

<p>gitlog.id just prints the patch SHA1 IDs:

```sh
$ gitlog.id
-----------------------[ prototype7 ]-----------------------
56908eeb6940 2ca4a6b628a1 fc64ad5d99fe 02031a00a251 f6f38da7dd18 d8546e8f0023 fc3cc1f98f6b 12c3e0cb3523 76cce178b134 6fc1dce3ab9c 1b681ab074ca 26fed8de719b 802ff51a5670 49f67a512d8c f04f20193bbb 5f6afe809d23 2030521dc70e dada79b3be94 9b19a1e08161 78a035041d3e f03da011cae2 0d2b2e068fcd 2449976aa133 57dfb5e12ccd 53abedfdcf72 6fbdda3474b3 49544a547188 187032f7a63c 6f75dae23d93 95fc2a261b00 ebfb14ded191 f653ee9e414a 0e2911cb8111 73968b76e2e3 8a3e4cb5e92c a5f2da803b5b 7c9ef68388ed 71ca19d0cba8 340d27a33895 9b3c4e6efb10 d2e8c22be39b 9563e31f8bfd ebac7a38036c f703a3c27874 a3e86d2ef30e da3c604755b0 4525c2f5b46f a06a5b7dea02 8ba93c796d5c e8b5ff851bb9
```

<br></br>
**<h2 style="text-align: center;">gitlog.id2</h2>**
<p>
gitlog.id2 is the same as gitlog.id but without the branch line at the top. This is handy for cherry-picking all patches from one branch to the current branch:

```sh
$ # create a new branch
$ git branch --track origin/master
$ # check out the new branch I just created
$ git checkout prototype9
$ # cherry-pick all patches from the old branch to the new one
$ for i in `gitlog.id2 prototype8` ; do git cherry-pick $i ;done
```

<br></br>
**<h2 style="text-align: center;">gitlog.grep</h2>**
<p>gitlog.grep greps for a string within that collection of patches. For example, if I find a bug and want to fix the patch that has a reference to function inode_go_sync, I can simply do:

```sh
$ gitlog.grep inode_go_sync
-----------------------[ prototype19 - 50 patches ]-----------------------
(snip)
11 340d27a33895 Nathan Nosudo     gfs2: drain the ail2 list after io errors
10 9b3c4e6efb10 Enkeli Gjylameti     gfs2: clean up iopen glock mess in gfs2_create_inode
 9 d2e8c22be39b Stelion Kontos     gfs2: Do proper error checking for go_sync family of glops
152:-static void inode_go_sync(struct gfs2_glock *gl)
153:+static int inode_go_sync(struct gfs2_glock *gl)
163:@@ -296,6 +302,7 @@ static void inode_go_sync(struct gfs2_glock *gl)
 8 9563e31f8bfd Nathan Nosudo gfs2: use page_offset in gfs2_page_mkwrite
 7 ebac7a38036c Nathan Nosudo gfs2: don't use buffer_heads in gfs2_allocate_page_backing
 6 f703a3c27874 Vitali Makaveli gfs2: Improve mmap write vs. punch_hole consistency
 5 a3e86d2ef30e Andreas Ellis gfs2: Multi-block allocations in gfs2_page_mkwrite
 4 da3c604755b0 Andreas Ellis gfs2: Fix end-of-file handling in gfs2_page_mkwrite
 3 4525c2f5b46f Enkeli Gjylameti     Vitali Makaveli's slab instrumentation
 2 a06a5b7dea02 Nathan Nosudo     GFS2: Add go_get_holdtime to gl_ops
 ^ 8ba93c796d5c Nathan Nosudo     gfs2: introduce new function remaining_hold_time and use it in dq
 H e8b5ff851bb9 Nathan Nosudo     gfs2: Allow rgrps to have a minimum hold time
```
So, now we know that patch HEAD~9 is the one that needs fixing. I use: 
```sh
git rebase -i HEAD~10
``` 
to edit patch 9,
```sh
 git commit -a --amend, 
```
then:
```sh 
 git rebase --continue  
```
to make the necessary adjustments.

<br></br>
**<h2 style="text-align: center;">coming-soon</h2>**
<p>

<br></br>
**<h2 style="text-align: center;">coming-soon</h2>**
<p>

<br></br>
**<h2 style="text-align: center;">fetchall</h2>**
<p>Process a directory of git repos, fetching all remotes and updating
 the default local branch.


<br></br>
**<h2 style="text-align: center;">updateall</h2>**
<p>Process a directory of mirrored git repos, updating from the remote.