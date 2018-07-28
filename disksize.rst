===========================
.. highlight:: shell

Aumentar o tamanho do Disco
===========================

1. Verifique o espaço ocupado de cada file system:

.. code-block:: console

    $ df -ha

1. Verifique o espaço livre no vgs:

.. code-block:: console
    
    # vgs

Extenda o tamanho da lvm:

.. code-block:: console

    # lvextend -l +100%FREE /dev/mapper/vg0-opt
    # # ou
    # lvextend -L+1G /dev/mapper/vg0-opt

Aumente o tamanho do filesystem:

.. code-block:: console

    # resize2fs /dev/mapper/vg0-opt

Caso ocorra o erro:

resize2fs: Bad magic number in super-block while trying to open /dev/mapper/system-usr
Couldn't find valid filesystem superblock.
Tente Executar: 

.. code-block:: console

    X xfs_growfs /dev/mapper/system-usr



Referencias
-----------

https://www.tecmint.com/extend-and-reduce-lvms-in-linux/