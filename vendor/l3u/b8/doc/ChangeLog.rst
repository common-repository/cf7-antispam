.. SPDX-FileCopyrightText: 2006-2022 Tobias Leupold <tl at stonemx dot de>

   SPDX-License-Identifier: CC-BY-SA-4.0

   The format of this file is inspired by keepachangelog.com, but uses ReStructuredText instead of
   MarkDown. Keep the line length at no more than 100 characters (with the obvious exception of the
   header template below, which needs to be indented by three spaces)

   Here's the header template to be pasted at the top after a new release:

   ====================================================================================================
   [unreleased]
   ====================================================================================================

   Added
   =====

   * for new features.

   Changed
   =======

   * for changes in existing functionality.

   Deprecated
   ==========

   * for soon-to-be removed features.

   Removed
   =======

   * for now removed features.

   Fixed
   =====

   * for any bug fixes.

   Security
   ========

   * in case of vulnerabilities.

====================================================================================================
[unreleased]
====================================================================================================

Added
=====

* Malte Paskuda contributed a PDO based SQLite storage backend.

Changed
=======

* Switched from "LGPL 2.1 only" to "LGPL 3.0 or later", in agreement with all authors.

* Switched to utf8mb4 for MySQL storage backend to support the full UTF-8 charset.

Deprecated
==========

* for soon-to-be removed features.

Removed
=======

* for now removed features.

Fixed
=====

* Fixed problem with `spl_autoload_register()` when using the Smarty PHP Template Engine
  Thanks to Malte Paskuda and Michael Loesler for pointing this out and providing a patch!

Security
========

* in case of vulnerabilities.

====================================================================================================
b8 0.7 released (18.03.2020)
====================================================================================================

* Removal: Removed the legacy HTML extractor from the lexer, along with the "old_get_html"
  configuration option.

* Change: The degenerator's "multibyte" option (that makes the degenerator use mb_strtolower,
  mb_strtoupper and mb_substr instead of strtolower, strtoupper and substr) now defaults to true.

* New: Added argument types to all function calls.

* New: Added abstract functions to the base storage class, so that implementing a custom one will be
  easier.

* Change: Added an spl_autoload_register() autoloader.

* Change: Due to namespaces restrictions, the default lexer and degenerator are not called "default"
  anymore but "standard".

* New: Introduced namespaces

* Removal: Removed the old db update scripts. It has been so long that probably, nobody still uses
  an old b8 installation anymore.

* Removal: Removed the PostgreSQL backend (due to lack of possibility of testing/maintaining).

* Removal: Removed the original MySQL backend using the long-deprecated mysql functions (as opposed
  to the newer mysqli ones).

* General: Overall code rework. The library needed it ;-) This includes:

  - Renamed functions to consistent names.

  - Added a "start transaction" function to the base storage backend.

  - Moved the database connection setup out of the backend. The user should care about this.

* Change: Switched to a more meaningful ChangeLog format.

====================================================================================================
b8 0.6.2 released (08.02.2019)
====================================================================================================

* b8.php: Removed the use of the (as of PHP 7.2) deprecated each() function. Thanks to Markus Hauser
  <mh@diebeiden.at> for the bug report along with a proposed patch!

* Overall code cleanup: Fixed coding style, indentation, function names, variable names ...

====================================================================================================
b8 0.6.1 released (12.03.2014)
====================================================================================================

* storage/storage_postgresql.php: Added a backend for PostgreSQL, written by Tom Regner
  <tom@goochesa.de>

* storage/storage_mysql.php: Fixed the theoretical possibility for an SQL injection. Thanks to Dirk
  Stolle for the bug report!

* storage/storage_mysqli.php: Added a MySQL backend using the new mysqli_* functions instead of the
  legacy mysql_* ones. Thanks to Lorenzo Masetti <lorenzo.masetti@gmail.com> for the initial port!

====================================================================================================
b8 0.6 released (30.11.2012)
====================================================================================================

* Lots of changes (new major release!):

  - Finally did an actually really complete abstraction of the storage backends, so that it was
    possible to change MySQL's database layout to multiple columns.

  - Kicked out the never-used lastseen parameter.

  - Renamed the internal variables to "b8*...", combined "bayes*text.ham" and "bayes*texts.spam" to
    "b8*texts".

  - Thus, the database layout has been changed. Included update scripts for both MySQL and DBA.

  - Removed all validate() functions in favor of throwing exceptions when something's wrong.

  - Made the lexer more flexible. Added functions for all split work, that, except for the raw
    split, can be turned on and off via a new config array.

  - The lexer now supports getting BBCode.

  - Added an additional check to the lexer to be sure no token will collide with an internal
    variable

  - Added multibyte support to the degenerator so it is now able to handle non-latin-1 texts in the
    same way as it handles latin-1-texts.

  - b8's constructor now takes four config arrays, the third is the lexer config, the fourth is the
    degenerator config.

* Bugfixes:

  - Removed the ucfirst function from the degenerator and replaced it with a custom one. It did not
    what I always thought it would do (first letter upper case, rest lower case), but does only
    converted the first letter to upper case.

  - Fixed the MySQL backend so it's now able to handle a get() request for an empty array or an
    array containing just one token.

  - Fixed the MySQL backend when doing a query with no returned result.

  - Fixed the lexer to never output an empty array of tokens, but a placeholder token if no token
    has been found.

====================================================================================================
b8 0.5.2 released (19.04.2011)
====================================================================================================

* storage/storage_mysql.php: The destructor of the MySQL storage backend always disconnected the
  MySQL connection, even if an existing link-resource was passed to b8. Now, the MySQL connection is
  only disconnected if b8 created the resource itself. Thanks to Gerhard Waldemair for the bug
  report!

====================================================================================================
b8 0.5.1 released (30.12.2010)
====================================================================================================

* Bigger changes:

  - Fixed some issues with the scope of variables leading to problems when multiple instances of b8
    are created. Thanks to Mike Creuzer for the bug report :-)

  - Centralized the loading of class definition files in the b8 constructor and created a function
    to handle the inclusion.

* b8.php: Return a lexer error code instead of a rating if the lexer failed. The lexer never
  returned FALSE but b8 checked only for this value to validate the lexer didn't fail. Thanks to
  Matt Friedman for the bug report :-)

* lexer/lexer_default.php: A bit of code cleanup: less useless nesting.

* doc/readme.*: Updated the documentation, added a FAQ.

====================================================================================================
b8 0.5-r1 released (27.06.2010)
====================================================================================================

* doc/readme.*: Updated the documentation; forgot the newly introduced b8::HAM and b8::SPAM
  variables. Added some additional information about the storage model.

====================================================================================================
b8 0.5 released (02.06.2010)
====================================================================================================

* 100.000 Changes (new major release!), at a glance:

  - No PHP 4 compatibility anymore. Much cleaner code base with less hacks.

  - Completely reworked storage model. The SQL performance increased dramatically, the Berkeley DB
    performance remains as fast as it always has been.

  - Better lexer which can also handle non-latin1 texts in a nice way, so that e. g. Cyrillic or
    Chinese texts can be classified more performant.

  - No config files anymore, multiple instances of b8 can be now created in the same script with
    different configuration, databases and no problems.

  - No spooky administration interface anymore that needs an SQL database, even if Berkeley DB is
    used (anybody who actually used this?! I never did ;-).

  - No "install" scripts and routines and a less end-user compatible documentation. Anybody
    integrating b8 in his homepage won't be an end-user, will he?

====================================================================================================
SVN Revision 221 (the original PHP 5 port 03.02.2009)
====================================================================================================

* Oliver Lillie (aka buggedcom) ported b8 to PHP 5:

  - Rewrote Tobias' original class for optimisation and PHP 5 functionality.

  - Improved database mysql query useage by over ~820%

  - Class is faster, ~20%.

  - Slight increase in memory usage, but it's small and given the advantages of the speed increase
    and query reduction it's worth it.

  - Removed install code from mysql class and added a sql file. Anyone who wants to use this is
    generally going to be more advanced anyway and see the sql to install.

====================================================================================================
b8 0.4.4 released (03.02.2009)
====================================================================================================

* Changed the license type from GPL to LGPL

====================================================================================================
b8 0.4.3 released (27.06.2008)
====================================================================================================

* No bugs found ... so let's make a release with only small changes ;-)

* b8.php: Removed debugging messages that were commented out anyway

* storage/storage_mysql.php: Made it possible to pass both a MySQL-link resource and a table name to
  b8. This makes b8 useable in the Redaxo CMS (and probably others)

* doc/readme.htm: Updated documentation accordingly

====================================================================================================
b8 0.4.2 released (17.02.2008)
====================================================================================================

* interface/backup.php: the bayes*dbversion tag is now written to a database emptied by drop(), so
  that it will be useable without an error message even if no backup is recovered afterwards.

* doc/readme.htm: added a security note to the configuration section (htaccess should be used to
  avoid everybody to be able to see the configuration)

====================================================================================================
b8 0.4.1 released (17.09.2007)
====================================================================================================

* storage/storage_mysql.php: fixed b8 crashing when getting passed a persistent MySQL resource link.
  Thanks to Paul Chapman for the bug report :-)

====================================================================================================
b8 0.4 released (08.06.2007)
====================================================================================================

* Let's go the whole hog. b8's class is now "b8" and no more "bayes", and all internal variables
  have now according names.

* Reworked the whole (surprisingly crappy) implementation of b8. No more global() calls, everything
  happens inside the classes now. Made that whole stuff really object oriented (as good as possible
  with PHP's poor OOP model ;-).

* No more PHP code in the configuration files.

* Created an extra lexer class. This is now also configurable.

* Storage classes now can create their own databases when this is requested by the configuration.

* MySQL calls are no random shots anymore: either, a MySQL-link resource is passed to b8 on startup
  which will be used for the queries, or the class sets up it's own link. Same for SQLite.

* The interface now uses a separate storage backend capable of SQL. In this way, we _really_ can
  query the database for e. g. an ordered list of tokens. After doing what we wanted with this work
  database, the b8 database can be synced with it.

* Added a lot of verbose error handling.

* Fixed a dumb error: all tokens from a text were used for the spamminess calculation, because two
  for() loops both used $i as their counter. D'oh!!! Now, the filter's performance is way better.

* Catched on the way how that whole math stuff works a little more ;-) Now, the calculation of the
  single probabilities proposed by Mr. Robinson does a little more the stuff it was intended to do,
  because ...

* Made some calculation constants parameters: the number of tokens to use, the default rating for
  unknown tokens and Gary Robinson's s constant.

* Introduced an optional minimum deviation that a token's rating must have to be considered in the
  spamminess calculation.

* The default extreme ratings for tokens only in ham or spam are now optional. One can also choose
  to calculate all ratings by Mr. Robinson's method.

* Noticed that text primary keys are not case sensitive by default in MySQL, which has a noticeable
  impact on the filter's performance. Informed the MySQL users about that.

* The whole code sucks much less ;-) b8 should be way more user friendly now.

* Re-wrote the whole documentation.

* Fixed the ChangeLog :-)

====================================================================================================
b8 0.3.3 released (08.02.2007)
====================================================================================================

* bayes-php is now b8. See http://www.nasauber.de/blog/text.php?text=58 for details :-) Thanks to
  Tobias Lang (http://langt.net/) for this cool new name!

====================================================================================================
bayes-php 0.3.3 released (05.01.2007)
====================================================================================================

* Renamed the internal BerkeleyDB handle from "$db" to the less general name "$bayes_php_db" due to
  an collision with phpwcms's (http://www.phpwcms.de/) global $db variable and potentially other php
  programs.

* Commented out Laurent Goussard's SQLite storage class by default, as it's try { } catch { } calls
  break PHP 4

====================================================================================================
bayes-php 0.3.2 released (03.09.2006)
====================================================================================================

* Laurent Goussard (loranger@free.fr) contributed an SQLite storage class(which needs PHP 5).

* I finally added my eMail address to the sources ;-)

====================================================================================================
bayes-php 0.3.1 released (24.07.2006)
====================================================================================================

* Fixed a problem in the unlearn() function: If a text was unlearned that wasn't learned before
  (accidentaly), it could happen that the count parameter for this text was smaller than 0, breaking
  the spamminess calulation

====================================================================================================
bayes-php 0.3 released (02.07.2006)
====================================================================================================

* Improved the get_tokens() function; the filter should now be a lot more performant, especially
  with short texts

* Added the "lastseen" parameter for each token to make the database maintainable (outdated tokens
  can be deleted)

* Added a real database maintainance interface

====================================================================================================
bayes-php 0.2.1 released (12.06.2012)
====================================================================================================

* Fixed a problem in get_tokens() (if it was called more than once, tokens were counted more often
  than they appeared in the text)

* Slightly enhanced the default index.php interface: after learning a text as Ham or Spam, the
  rating before and after it is displayed to inform the user about it

====================================================================================================
bayes-php 0.2 released (21.05.2006)
====================================================================================================

* Comments now in English (to pretend international success of bayes-php ;-)

* Recommendations of Paul Graham's article "Better Bayesian Filtering"
  ( http://www.paulgraham.com/better.html ) are now considered: Tokens that only appear in Ham or
  Spam and not in the other category are rated with 0.9998 or 0.0002 if they were less than 10 times
  in Ham or Spam and with 0.9999 or 0.0001 if they appeared more that 10 times. This should allow
  the filter to differentiate spam texts more sharp from ham texts. Also, token "degeneration" as
  described in the article is performed for unknown tokens to estimate their spamminess.

* The database connect is now swapped in a separate configuration file, so only this file has to be
  preserved if bayes-php is updated and only this file has to be changed to configure the script.

====================================================================================================
bayes-php 0.1.1 released (29.03.2006)
====================================================================================================

* get_tokens() beachtet jetzt auch HTML-Tags und Wörter mit Akzenten und Apostrophen

* Verschiedene Kleinigkeiten "sauber" gemacht :-)

====================================================================================================
bayes-php 0.1 released (05.03.2006)
====================================================================================================

* Erstes Release
