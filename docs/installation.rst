Installation
==============

Automated Installation
~~~~~~~~~~~~~~~~~~~~~~
This installation procedure assumes that you have a working perl version and a `Miniconda`_
installation.

1. "Install" MIP

  .. code-block:: console
    
    clone the official git repository
    $ git clone https://github.com/henrikstranneheim/MIP.git
    $ cd MIP

  After this you can decide whether to make MIP an "executable" by either adding the install directory to the ``$PATH`` in e.g. "``~/.bash_profile``" or move all the files from this directory to somewhere already in your path like "``~/usr/bin``". 
  Remember to make the file(s) executable by ``chmod +x file``.
  
2. Create the install instructions for MIP

  .. code-block:: console
  
    $ perl install.pl
    This will generate a batch script "mip.sh" for the install in your working directory.
    
3. Run the bash script

  .. code-block:: console
 
    $ bash mip.sh
    This will install all the dependencies of MIP and other modules included in MIP into a conda environment (defaults to "mip"). 
    However a fresh version of perl and cpanm is installed outside of the conda environment, but are activated through bashrc and  bash_profile.

.. note::

  This will add the following lines to bashrc and bash_profile if the install perl version 
  is not found in your path:
  
  .. code-block:: console
  
     'export PATH=$HOME/perl-PERLVERSION/:$PATH' >> ~/.bashrc
     'eval `perl -I ~/perl-PERLVERSION/lib/perl5/ -Mlocal::lib=~/perl-PERLVERSION/`' >> ~/.bash_profile
     'export PERL_UNICODE=SAD' >> ~/.bash_profile

3. Run MIP

  .. code-block:: console
    
    $ source activate mip
    $ mip.pl -h


Manual Installation
~~~~~~~~~~~~~~~~~~~~

1. Install a fresh copy of Perl

  On UNIX, Perl5 can be installed by following these `instructions <http://learn.perl.org/installing/unix_linux.html>`_. It uses `Perlbrew <http://perlbrew.pl/>`_.

2. To switch to the new Perl installation, you might need to run:

  .. code-block:: console
    
    $ INSTALLER_PERL_VERSION=5.16.0
    $ perlbrew switch perl-$INSTALLER_PERL_VERSION

3. "Install" MIP

  .. code-block:: console
    
    clone the official git repository
    $ git clone https://github.com/henrikstranneheim/MIP.git
    $ cd MIP
    $ perl mip.pl -h

  After this you can decide whether to make MIP an "executable" by either adding the install directory to the ``$PATH`` in e.g. "``~/.bash_profile``" or move all the files from this directory to somewhere already in your path like "``~/usr/bin``". 
  Remember to make the file(s) executable by ``chmod +x file``.
  
4. Dependencies

  You need to make sure all depedencies are installed and loaded (See :doc:`setup`). 
  However, MIP should tell you if something is missing.

5. To install the dependencies - use cpanm:

  .. code-block:: console
    
    cpanm <dependency>
    $ cpanm YAML

.. _Miniconda: http://conda.pydata.org/miniconda.html