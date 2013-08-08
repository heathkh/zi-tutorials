.. _publish_src_packages:

******************************
How To Publish Source Packages
******************************

To publish feeds, you need to setup a repository with 0repo.  To do this, first complete the tutorial for how to :ref:`setup your repository. <setup_repository>`.


Start with a empty feed template xml

.. code-block:: xml 
  
  <?xml version="1.0"?>
  <interface xmlns="http://zero-install.sourceforge.net/2004/injector/interface">
  <name>NAME</name>
  <summary>ONE LINE SUMMARY</summary>
  <description>MULTI-LINE DESCRIPTION</description>
  <homepage>URL TO PROJECT HOMEPAGE</homepage>

  <feed-for interface="INTERFACE_URL"/>
  <group released="DATE_STRING" stability="STABILITY_STRING" license="LICENSE_STRING">
    <command name="compile" shell-command="COMPILE_CMD_HERE">
      <compile:implementation xmlns:compile="http://zero-install.sourceforge.net/2006/namespaces/0compile"></compile:implementation>
    </command>
    <implementation arch="*-src" version="{version}">
      <manifest-digest/>
      <archive href="URL_TO_SOURCE_TARBALL"/>
    </implementation>
  </group>
  </interface>


For example, for the snappy library you would create a feed template file ``snappy.xml.template``

.. code-block:: xml 
  
  <?xml version="1.0"?>
  <interface xmlns="http://zero-install.sourceforge.net/2004/injector/interface">
  <name>snappy</name>
  <summary>A fast compressor/decompressor</summary>
  <description>
    Snappy is a compression/decompression library. It does not aim for maximum compression, or compatibility with any other compression library; instead, it aims for very high speeds and reasonable compression. For instance, compared to the fastest mode of zlib, Snappy is an order of magnitude faster for most inputs, but the resulting compressed files are anywhere from 20% to 100% bigger. On a single core of a Core i7 processor in 64-bit mode, Snappy compresses at about 250 MB/sec or more and decompresses at about 500 MB/sec or more.
  </description>
  <homepage>https://code.google.com/p/snappy</homepage>
  <feed-for interface="http://zeroinstall.dasgizmo.net/snappy.xml"/>
  <group released="2013-02-05" stability="stable" license="OSI Approved :: BSD License">
    <command name="compile" shell-command="&quot;$SRCDIR/configure&quot; --prefix=&quot;$DISTDIR&quot; &amp;&amp; make install">
      <compile:implementation xmlns:compile="http://zero-install.sourceforge.net/2006/namespaces/0compile">
      </compile:implementation>
    </command>
    <implementation arch="*-src" version="{version}">
      <manifest-digest/>
      <archive href="http://zeroinstall.dasgizmo.net/archives/snappy-{version}.tar.gz"/>
    </implementation>
  </group>
  </interface>


-------------------------------------

Create a source feed file from the template file

.. code-block:: bash

   0template snappy.xml.template version=1.1.0

This produces the file  ``snappy-1.1.0.xml``


-------------------------------------

Test that you can successfully build from source.

.. code-block:: bash

   0compile -c setup snappy-1.1.0.xml
   cd snappy-1.1.0
   0compile -c setup
   0compile build
   
-------------------------------------

Add as a new feed using 0repo.

Copy the feed file to the ``incoming`` directory of your 0repo install.

.. code-block:: bash

   cp snappy-1.1.0.xml  $HOME/repo/incoming
   cd $HOME/repo
   0repo update
   
Now check that your feed catalog includes the new source package.

http://<your_0repo_base_url>/catalog.xml

TODO: include screenshot here

What to do next
---------------

At this point, you may want to package up the binary you compiled from source
to provide others with a binary version for your platform.  See the :ref:`publish a binary package <how_to_publish_bin_packages>`.





