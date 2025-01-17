.. _upgrade:

Upgrading
---------

In any case before upgrading a major version read the document
`READ_BEFORE_UPDATE`_
which is continuously updated in the Github repository.
Note, that when you are upgrading over several major versions, read all the comments
for all versions.

If you installed eduMFA via DEB or RPM repository you can use the normal
system ways of *apt-get*, *aptitude* and *yum* to upgrade eduMFA to the
current version.

If you want to upgrade an old Ubuntu installation from eduMFA 2.23 to
eduMFA 3.0, please read the :ref:`Note on legacy upgrades <upgrade_packaged_legacy>`.

Different upgrade processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Depending on the way eduMFA was installed, there are different recommended update procedures.
The following section describes the process for pip installations.
Instructions for packaged versions on Ubuntu are found in :ref:`upgrade_packaged`.

Upgrading a pip installation
............................

If you install eduMFA into a python virtualenv like */opt/eduMFA*,
you can follow this basic upgrade process.

First you might want to backup your program directory:

.. code-block:: bash

   tar -zcf eduMFA-old.tgz /opt/eduMFA

and your database:

.. code-block:: bash

   source /opt/eduMFA/bin/activate
   edumfa-manage backup create

Running upgrade
^^^^^^^^^^^^^^^

Starting with version 2.17 the script ``eduMFA-pip-update`` performs the
update of the python virtualenv and the DB schema.

Just enter your python virtualenv (you already did so, when running the
backup) and run the command:

   eduMFA-pip-update

The following parameters are allowed:

``-f`` or ``--force`` skips the safety question, if you really want to update.

``-s`` or ``--skipstamp`` skips the version stamping during schema update.

``-n`` or ``--noschema`` completely skips the schema update and only updates the code.


Manual upgrade
^^^^^^^^^^^^^^

Now you can upgrade the installation:

.. code-block:: bash

   source /opt/eduMFA/bin/activate
   pip install --upgrade eduMFA

Usually you will need to upgrade/migrate the database:

.. code-block:: bash

   eduMFA-schema-upgrade /opt/eduMFA/lib/eduMFA/migrations

Now you need to restart your webserver for the new code to take effect.

.. _upgrade_packaged:

Upgrading a packaged installation
.................................

In general, the upgrade of a packaged version of eduMFA should be done using the
default tools (e.g. apt and yum). In any case, read the
`READ_BEFORE_UPDATE`_
file. It is also a good idea to backup your system before upgrading.

Ubuntu upgrade
^^^^^^^^^^^^^^

If you use the Ubuntu packages in a default setup, the upgrade can should be done
using::

   apt update
   apt dist-upgrade


.. _upgrade_packaged_legacy:

.. note::
    In case you upgrade from the old eduMFA 2.23.x to the version 3.x you have to
    change from your ppa sources to the new repositories. If you are upgrading your
    Ubuntu release, e.g. from 14.04 to 16.04 the principal steps are

    * Bring your Ubuntu 14.04 system up-to-date
    * Run the release upgrade (do-release-upgrade)
    * Eventually remove old repositories and add recent repositories as described in :ref:`add_ubuntu_repository`.
    * Reinstall/Upgrade eduMFA 3.x

    eduMFA 2.x installed the python packages to the system directly. The packages
    in the repository instead come with a virtual python environment. This may cause lots
    of obsolete packages after upgrading which may be removed with::

       apt autoremove
