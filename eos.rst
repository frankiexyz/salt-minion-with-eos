.. _installation-airsta-eos:

======
Arista EOS
======

.. _installation-eos:

Installation from the Official SaltStack Repository
===================================================

Packages for Arista EOS 4.17 and 4.18 are available in the SaltStack repository.

Download the swix package and save it in extension 
.. code-block:: bash
veos#copy https://image/salt-eos.swix extension:

.. _eos-install-pkgs:

Install Packages
================

Install the Salt minion or other packages from the repository with


Copy the Salt package to extension
.. code-block:: bash
veos#copy flash:salt-eos-2017-07-19.swix extension:

Install the swix
.. code-block:: bash
veos#extension salt-eos.swix force

Verify the installation
.. code-block:: bash
veos#show extensions | i salt-eos      
salt-eos-2017-07-19.swix      1.0.11/1.fc25        A, F                27   

Change the Salt master by edit the variable(SALT_MASTER)
.. code-block:: bash
veos#bash vi /mnt/flash/startup.sh

Make sure you enable the api with unix-socket 
.. code-block:: bash
veos(config)#management api http-commands
             protocol unix-socket
             no shutdown

Generate Keys and host record and start Salt minion
.. code-block:: bash
veos#bash sudo /mnt/flash/startup.sh

salt-minion should be running
.. code-block:: bash
veos#bash ps -ef |grep salt
root     10939     1  3 20:17 ?        00:00:00 /usr/bin/python /usr/bin/salt-minion -d
root     10941 10939  0 20:17 ?        00:00:00 /usr/bin/python /usr/bin/salt-minion -d
admin    10983 10945  0 20:18 pts/8    00:00:00 grep --color=auto salt

Copy the installed extensions to boot-extensions
.. code-block:: bash
veos#copy installed-extensions boot-extensions 

Apply event-handler to let EOS start salt-minion during boot-up 
.. code-block:: bash
veos(config)#event-handler boot-up-script       
   trigger on-boot                 
      action bash sudo /mnt/flash/startup.sh



.. _eos-config:

Post-installation tasks
=======================

Now go to the :ref:`Configuring Salt<configuring-salt>` page.
