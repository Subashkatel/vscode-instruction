Access DSMLP Using VSCode
============================

1. Create SSH Key and Add to DSMLP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Create an SSH Key on your local machine and append the public key to DSMLP's ``~/.ssh/authorized_keys`` file. Follow the steps in this `link <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=mac#about-ssh-key-passphrases>`_ to generate a key pair (Follow the guide till step 3 under 'Adding your SSH key to the ssh-agent').

After generating the key, append it to DSMLP using:

.. code-block:: bash

    ssh-copy-id -i ~/.ssh/id_ed25519 your-username@dsmlp-login.ucsd.edu

Make sure to replace ``your-username`` with your actual UCSD username.

2. Install VS Code
^^^^^^^^^^^^^^^^^^
Install VS Code if you haven't already: `VS Code Download <https://code.visualstudio.com/download>`_

3. Install Remote-SSH Extension
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Install Remote-SSH plugin by searching for it in the extensions view.
Extension Identifier: `` ms-vscode-remote.remote-ssh``

4. Configure SSH Connection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. Click on the indicator on the bottom left corner

    .. image:: vscode_indictor.png
        :alt: VS Code SSH Configuration
        :width: 600px


2. Click on ``Connect to Host...``
3. Click on ``Configure SSH Hosts...``
4. Click on the ``Users/name/.ssh/config``
5. Add the following lines (make sure to change ``your-username`` to your username):

.. code-block:: text

    Host vscode-dsmlp
    HostName dsmlp-login.ucsd.edu
    HostKeyAlias vscode-dsmlp
    IdentitiesOnly yes
    User your-username
    ProxyCommand ssh -i ~/.ssh/id_ed25519 your-username@dsmlp-login.ucsd.edu /opt/launch-sh/bin/launch-cse160-opencl-ssh.sh

6. Save the configuration
7. Click on the >< key at the bottom left corner and then click on ``Connect to Host...``
8. You should see a ``vscode-dsmlp`` option. Click on it to start your session.
9. You may be asked to insert the passphrase you created - do that and happy coding!

Important Notes
^^^^^^^^^^^^^^^
You already have access to GPU infrastructure on DSMLP; i.e. it starts a container with GPU access and loads it with a software image that contains CUDA and other basic packages. You must be within GPU container in order to properly compile. If you get an error about not having access to nvcc, then you are not in the container. Please only use the container when you are compiling and release it when you are completed.

Note
^^^^
When you close VSCode, the kubernetes pod is not released automatically. You have to manually delete the pod using:

.. code-block:: bash

    kubectl delete pod <pod_name>

To find your pod's name, you can run:

.. code-block:: bash

    kubectl get pods

and find all the pods open. Once you do this, you will be able to use the terminal for launching a new container if you want.
