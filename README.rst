Passhole
========

Passhole is a CLI interface for KeePass 1.x (v3) and 2.x (v4) databases with support for dmenu.

Passhole allows you to edit and generate passwords much in the same way as the popular `pass`_ utility.

.. _pass: https://www.passwordstore.org

Passhole can also generate `correct horse battery staple`_ style passwords, which have plenty of entropy (when using 5 or more words) and are easier to type out manually than random alphanumeric passwords.

.. _correct horse battery staple: http://xkcd.com/936

Dependencies
------------

- PyUserInput
- pykeepass
- gnupg

Installation
------------

Install through pip

.. code:: bash

   pip install passhole

Or latest

.. code:: bash

   pip install -e git+https://github.com/purdueLUG/passhole.git#egg=passhole

Example Usage
--------------

Note: This program uses GPG to store temporary files, you need to have a GPG key available

.. code:: bash

   # initialize the database
   >>> passhole init
   Creating database at /home/evan/.passhole.kdbx
   Enter your desired database password
   Password: 
   Confirm:

   # add a new entry with manually created password
   >>> passhole add github
   Username: Evidlo
   Password: 
   Confirm: 
   URL: github.com

   # add an entry with a generated alphanumeric password
   >>> passhole add neopets -a
   Username: Evidlo
   URL: neopets.com

   # add a new group
   >>> passhole add social/
   
   # add an entry to `social/` with a 32 character password (alphanumeric + symbols)
   >>> passhole add social/facebook -s 32

   # add an entry to `social/` with a correct-horse-battery-staple type password
   >>> passhole add social/twitter -w

   # list all entries
   >>> passhole list
   github
   neopets
   [social]
   ├── facebook
   └── twitter

   # display contents of entry
   >>> passhole show social/twitter
   Title: twitter
   Username: Evidlo
   Password: inns-ambien-travelling-throw-force
   URL: twitter.com

   # select entry using dmenu, then send password to keyboard
   >>> passhole type dmenu
   inns-ambien-travelling-throw-force

   # select entry using dmenu, then send username and password to keyboard, separated by a tab
   >>> passhole type dmenu --tabbed
   Evidlo	inns-ambien-travelling-throw-force

Troubleshooting
---------------

.. code:: python

   File "~/passhole/passhole/passhole.py", line 47, in <module>
     default_key = next(c.keylist())
   StopIteration
   
GPG key doesn't exist or is not loaded in GPG agent

.. code:: python

   Passsword or keyfile incorrect

If you are not using a keyfile, it can be your password cache that is incorrect. You can try some of the following options:

- ``--nocache`` : it should prompt for the password
- ``--nokeyfile`` : it should skip trying to open the db with a keyfile
- delete `~/.cache/passhole_cache` : it's an encrypted file containing the db password

