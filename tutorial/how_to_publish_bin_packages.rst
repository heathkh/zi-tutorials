.. _publish_bin_packages:

******************************
How To Publish Binary Packages
******************************

First, publish a source package by following the tutorial for how to :ref:`publish a source package <publish_src_packages>`.



-------------------------------------

Test that you can successfully build from source.

.. code-block:: bash

   0compile -c setup http://zeroinstall.dasgizmo.net/snappy.xml
   cd snappy
   0compile -c setup
   0compile build
   
-------------------------------------


Create the binary package to be uploaded to the server and the xml snippet to add to the feed.

.. code-block:: bash

   0compile publish http://zeroinstall.dasgizmo.net/archives
   
Copy the tarball to the server

.. code-block:: bash
     
   cp snappy-linux-x86_64-1.1.0.tar.bz2 /var/www/zeroinstall/archives/
   
 Next we need to update the repository feed file to include the new implementation.

 Copy the "implementation" portion from the ``snappy-1.1.0.xml`` created when you ran the ``0compile publish`` command.

.. code-block:: xml

  <group arch="Linux-x86_64">
    <implementation id="sha1new=06c387ae0fafc56bf4d682dd8a7d7f4e49d6d274" released="2013-08-07" version="1.1.0">
      <manifest-digest sha256new="VNEAOHELYHO74ZB4GGYLVK6PDHLGN46WODKEC5RSQAF2HZP23HFQ"/>
      <archive extract="snappy-linux-x86_64-1.1.0" href="http://zeroinstall.dasgizmo.net/archives/snappy-linux-x86_64-1.1.0.tar.bz2" size="140408"/>
    </implementation>
  </group>

Next paste this snippet into the ``snappy.xml`` feed in the feeds directory of your 0repo server (this assumes you followed the previous tutorial and a source feed already exists).

.. code-block:: bash
   
   cd $HOME/repo/feeds
   vim snappy.xml
   # now paste the snippet you copied above so the file looks like this
     
.. code-block:: xml 
  
   <?xml version="1.0" ?>
   <interface uri="http://zeroinstall.dasgizmo.net/snappy.xml" xmlns="http://zero-install.sourceforge.net/2004/injector/interface">
  <name>snappy</name>
  <summary>A fast compressor/decompressor</summary>
  <description>
   Snappy is a compression/decompression library. It does not aim for maximum compression, or compatibility with any other compression library; instead, it aims for very high speeds and reasonable compression. For instance, compared to the fastest mode of zlib, Snappy is an order of magnitude faster for most inputs, but the resulting compressed files are anywhere from 20% to 100% bigger. On a single core of a Core i7 processor in 64-bit mode, Snappy compresses at about 250 MB/sec or more and decompresses at about 500 MB/sec or more.
  </description>

  <homepage>https://code.google.com/p/snappy</homepage>

  <group license="OSI Approved :: BSD License" released="2013-02-05" stability="stable">
    <command name="compile" shell-command="&quot;$SRCDIR/configure&quot; --prefix=&quot;$DISTDIR&quot; &amp;&amp; make install">
      <compile:implementation xmlns:compile="http://zero-install.sourceforge.net/2006/namespaces/0compile"></compile:implementation>
    </command>

    <implementation arch="*-src" id="sha1new=5e1616a6cc21024d1bb35957d9fabe55a2b79b83" version="1.1.0">
      <manifest-digest sha256new="HPZOI5ZC5L6TJW5GENQUVXI2G57LUL2XACXKFOXRLTRJZ4QUVAZA"/>
      <archive extract="snappy-1.1.0" href="http://zeroinstall.dasgizmo.net/archives/snappy-1.1.0.tar.gz" size="1719945"/>
    </implementation>

    <group arch="Linux-x86_64">
    <implementation id="sha1new=06c387ae0fafc56bf4d682dd8a7d7f4e49d6d274" released="2013-08-07" version="1.1.0">
      <manifest-digest sha256new="VNEAOHELYHO74ZB4GGYLVK6PDHLGN46WODKEC5RSQAF2HZP23HFQ"/>
      <archive extract="snappy-linux-x86_64-1.1.0" href="http://zeroinstall.dasgizmo.net/archives/snappy-linux-x86_64-1.1.0.tar.bz2" size="140408"/>
    </implementation>
    </group>

  </group>
  </interface>


Next tell 0repo you modified the feed by committing the changes to the internal git repository.

.. code-block:: bash   

  git commit -a


Next tell 0repo to update the catalog

.. code-block:: bash   

  cd $HOME/repo
  0repo update


Now if you check the feed url, you'll see the source AND binary packages.

.. image:: /_static/snappy_source_and_binary_versions.png
   :width: 100%
   :align: center


