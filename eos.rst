
=========================================
Arista EOS Salt minion installation guide
=========================================

Important
=========

This swix file is built by the following packages

.. code-block::

      libsodium-1.0.11-1.fc25.i686.rpm
      libstdc++-6.2.1-2.fc25.i686.rpm
      openpgm-5.2.122-6.fc24.i686.rpm
      python-Jinja2-2.8-0.i686.rpm
      python-PyYAML-3.12-0.i686.rpm
      python-babel-0.9.6-5.fc18.noarch.rpm
      python-backports-1.0-3.fc18.i686.rpm
      python-backports-ssl_match_hostname-3.4.0.2-1.fc18.noarch.rpm
      python-backports_abc-0.5-0.i686.rpm
      python-certifi-2016.9.26-0.i686.rpm
      python-chardet-2.0.1-5.fc18.noarch.rpm
      python-crypto-1.4.1-1.noarch.rpm
      python-crypto-2.6.1-1.fc18.i686.rpm
      python-futures-3.1.1-1.noarch.rpm
      python-jtextfsm-0.3.1-0.noarch.rpm
      python-kitchen-1.1.1-2.fc18.noarch.rpm
      python-markupsafe-0.18-1.fc18.i686.rpm
      python-msgpack-python-0.4.8-0.i686.rpm
      python-napalm-base-0.24.3-1.noarch.rpm
      python-napalm-eos-0.6.0-1.noarch.rpm
      python-netaddr-0.7.18-0.noarch.rpm
      python-pyeapi-0.7.0-0.noarch.rpm
      python-salt-2017.7.0_1414_g2fb986f-1.noarch.rpm
      python-singledispatch-3.4.0.3-0.i686.rpm
      python-six-1.10.0-0.i686.rpm
      python-tornado-4.4.2-0.i686.rpm
      python-urllib3-1.5-7.fc18.noarch.rpm
      python2-zmq-15.3.0-2.fc25.i686.rpm
      zeromq-4.1.4-5.fc25.i686.rpm


Installation from the Official SaltStack Repository
===================================================

Packages for Arista EOS 4.17 and 4.18 are available in the SaltStack repository.

Download the swix package and save it in flash 

.. code-block:: 

   veos#copy https://salt-eos.netops.life/salt-eos-latest.swix flash:
   veos#copy https://salt-eos.netops.life/startup.sh flash:

Install Packages
================


Copy the Salt package to extension

.. code-block::

   veos#copy flash:salt-eos-latest.swix extension:

Install the swix
.. code-block::

   veos#extension salt-eos-latest.swix force

Verify the installation
.. code-block::

    veos#show extensions | i salt-eos      
         salt-eos-2017-07-19.swix      1.0.11/1.fc25        A, F                27   

Change the Salt master by edit the variable(SALT_MASTER)
.. code-block::

    veos#bash vi /mnt/flash/startup.sh

Make sure you enable the api with unix-socket 
.. code-block:: 

    veos(config)#management api http-commands
             protocol unix-socket
             no shutdown


Post-installation tasks
=======================
Generate Keys and host record and start Salt minion
.. code-block:: 

   veos#bash 
   #sudo /mnt/flash/startup.sh

salt-minion should be running

Copy the installed extensions to boot-extensions
.. code-block:: 

   veos#copy installed-extensions boot-extensions 

Apply event-handler to let EOS start salt-minion during boot-up 
.. code-block:: bash

   veos(config)#event-handler boot-up-script       
      trigger on-boot                 
      action bash sudo /mnt/flash/startup.sh

Now go to the :ref:`Configuring Salt<configuring-salt>` page.
