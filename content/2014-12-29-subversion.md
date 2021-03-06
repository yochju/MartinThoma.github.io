---
layout: post
title: Subversion
author: Martin Thoma
date: 2014-12-29 20:57
category: Code
tags: cheat sheet
featured_image:
---

<div class="info">This is a quick article I had for quite a while as a draft. It might not be finished or have other problems, but I still want to share it.</div>

Subversion or Apache Subversion or short svn is a is a software versioning and a revision control system. This allows you to step back to any point of the software and to develop simultaniously on the same project (but not on the same file). You have to make sure that no dependencies are broken, of course.

Here is a little cheat sheet how to use SVN. I will demonstrate some on my Google Code OpenSource project <a href="https://code.google.com/p/community-chess/">Community Chess</a>.

All commands are executed while I'm in my local working copy of the repository.

<h2>Get the repository</h2>

```bash
svn checkout https://community-chess.googlecode.com/svn/trunk/ community-chess --username themoosemind@googlemail.com
```


<h2>Update to the latest version</h2>

```bash
svn update
```


<h2>Check for changes</h2>

If you only want to check which files were modified, added or deleted you can
execute

```bash
svn status
```

This will show your local changes.


<h2>File actions</h2>
The actions are simmilar to the console commands, but you have to add svn:

<h3>Copy</h3>

```bash
svn cp myFile.php copiedFile.php
```


<h3>Delete</h3>

```bash
svn rm myFile.php
```

<h3>Rename / Move</h3>
```bash
svn mv myFile.php folder/myNewFile.php
```

<h3>Add file</h3>
If you created a file and want to submit it with the next commit, just do

```bash
svn add myFile.php
```

<h2>Commit the latest changes</h2>
<h3>Check your changes</h3>
First you should try <a href="#Check_for_changes-3">svn status</a>. What files did you change? Do you really want to upload those changes?

If svn status gives you an exclamation mark (!), you might have deleted a file which you wanted to add before. No problem. Just make

```bash
svn rever /path/to/your/file.php
```

<h3>I'm fine: Upload it!</h3>
This is the command you use, if you want to send the changes you made on your working copy to the repository:

```bash
svn commit -m "Moved some functions to additional.inc.php to keep the project more flexible; Much work for tournament implementation done; Some Warnings fixed" --username themoosemind@gmail.com
```

<h2>Watch changes</h2>
<h3>General</h3>
See the last three changes:

```bash
svn log -v --limit=3
```

```text
------------------------------------------------------------------------
r118 | themoosemind@gmail.com | 2011-09-26 23:18:42 +0200 (Mo, 26. Sep 2011) | 1 Zeile
Geänderte Pfade:
   M /trunk/clients/Java/ChessClient.class
   M /trunk/clients/Java/ChessClient.java

small changes on the Java Client
------------------------------------------------------------------------
r116 | themoosemind@gmail.com | 2011-09-26 22:53:01 +0200 (Mo, 26. Sep 2011) | 1 Zeile
Geänderte Pfade:
   A /trunk/clients/Java
   A /trunk/clients/Java/ChessClient.class
   A /trunk/clients/Java/ChessClient.java
   A /trunk/install/phpBB3
   A /trunk/install/phpBB3/login.wrapper.php (von /trunk/install/phpbb3.login.php:100)
   A /trunk/install/phpBB3/phpbb3.integration.txt (von /trunk/install/phpbb3.integration.txt:113)
   A /trunk/install/phpBB3/phpbb3.php (von /trunk/install/phpbb3.php:100)
   A /trunk/install/phpBB3/phpbb3.sql (von /trunk/install/phpbb3.sql:103)
   A /trunk/install/phpBB3/wrapper.inc.php (von /trunk/install/phpbb3.wrapper.inc.php:113)
   D /trunk/install/phpbb3.integration.txt
   D /trunk/install/phpbb3.login.php
   D /trunk/install/phpbb3.php
   D /trunk/install/phpbb3.sql
   D /trunk/install/phpbb3.wrapper.inc.php

Made integration instructions for phpBB easier; added Java-Client
------------------------------------------------------------------------
r113 | themoosemind@gmail.com | 2011-09-26 08:29:15 +0200 (Mo, 26. Sep 2011) | 1 Zeile
Geänderte Pfade:
   M /trunk/index.php
   M /trunk/install/chess.sql
   M /trunk/install/phpbb3.integration.txt
   M /trunk/install/phpbb3.wrapper.inc.php
   M /trunk/my_software.php
   M /trunk/wrapper.inc.php

Fixed issue 3 and some more phpBB-Bugs; more detailed integration instructions;Renamed GAMES_THREEFOLD_REPETITION_TABLE
------------------------------------------------------------------------
```

<h3>One specific file</h3>
I don't want to get that much output, so I don't apply -v this time:

```bash
svn log index.php --limit=3
```

```text
------------------------------------------------------------------------
r127 | themoosemind@gmail.com | 2011-10-04 00:09:59 +0200 (Di, 04. Okt 2011) | 1 Zeile

Moved some functions to additional.inc.php to keep the project more flexible; Much work for tournament implementation done; Some Warnings fixed
------------------------------------------------------------------------
r113 | themoosemind@gmail.com | 2011-09-26 08:29:15 +0200 (Mo, 26. Sep 2011) | 1 Zeile

Fixed issue 3 and some more phpBB-Bugs; more detailed integration instructions;Renamed GAMES_THREEFOLD_REPETITION_TABLE
------------------------------------------------------------------------
r71 | themoosemind@gmail.com | 2011-08-26 23:13:37 +0200 (Fr, 26. Aug 2011) | 1 Zeile

completed MVC with Vemplator; controlled every php file with phpcs
------------------------------------------------------------------------
```
