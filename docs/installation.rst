Installation
==============

Automated Installation
~~~~~~~~~~~~~~~~~~~~~~
This installation procedure assumes that you have a working perl version (>= 5.10) and a `Miniconda`_
installation.

1. "Install" MIP

  .. code-block:: console
    
    Clone the official git repository
    $ git clone https://github.com/henrikstranneheim/MIP.git
    $ cd MIP

*Optional*

  .. code-block:: console
    
    Test conda and mip_install
    $ cd t; prove mip_install.t
    $ cd -

2. Create the install instructions for MIP

  .. code-block:: console
  
    $ perl mip_install.pl

This will generate a batch script "mip.sh" for the install in your working directory. Use ``--help`` to see
parameters that can be used in the installation process. 

*Conda* 

You can decide to install in the conda default environment or use a conda environment with ``--env [env_name]``.
If you have installed conda in another location than the default you have to supply the path to the location
using ``--conda_dir_path [conda_directory_path]``.

*Perl*

MIP requires perl version (>=5.18) and the installation process will upgrade the perl version to at least 5.18 for the user
if you enable ``--perl_install``. Cpanm will be installed if you install a new perl version and used to download required 
perl modules. Currently MIP does not use the conda perl installation, but installs perl and cpanm outside of conda.

.. note::

  This will add the following lines to bashrc and bash_profile if the install perl version 
  is not found in your path:
  
  .. code-block:: console
  
     'export PATH=$HOME/perl-PERLVERSION/:$PATH' >> ~/.bashrc
     'eval `perl -I ~/perl-PERLVERSION/lib/perl5/ -Mlocal::lib=~/perl-PERLVERSION/`' >> ~/.bash_profile
     'export PERL_UNICODE=SAD' >> ~/.bash_profile

*References*

MIP requires many references depending on what modules in MIP you decide to run. MIP ships with a download script
that will attempt to download references that are available in public repositories. This feature can be enables with
by supplying a ``--reference_dir [reference_dir]`` in the installation process.

.. note::

  Some references are quite large and will take time to download.

3. Run the bash script

  .. code-block:: console
 
    $ bash mip.sh
    This will install all the dependencies of MIP and other modules included in MIP into a conda environment. 
    However a fresh version of perl and cpanm is installed, if enabled, outside of the conda environment, but are activated through bashrc and bash_profile.

*Optional*

Make sure to activate your conda environment if that option was used above.

  .. code-block:: console
    
    Test Perl modules and MIP
    $ cd t; prove run_tests.t
    $ cd -
    
3. Run MIP

*Conda default environment*

.. code-block:: console
    
    $ mip

*Conda environment*

  .. code-block:: console
    
    $ source activate conda_env
    $ mip


Manual Perl Installation
~~~~~~~~~~~~~~~~~~~~~~~~

We recommend using the automated installation procedure, but you can also install Perl manually.

1. Install a fresh copy of Perl

  On UNIX, Perl5 can be installed by following these `instructions <http://learn.perl.org/installing/unix_linux.html>`_. It uses `Perlbrew <http://perlbrew.pl/>`_.

2. To switch to the new Perl installation, you might need to run:

  .. code-block:: console
    
    $ INSTALLER_PERL_VERSION=5.18.0
    $ perlbrew switch perl-$INSTALLER_PERL_VERSION
  
3. Dependencies

  You need to make sure all depedencies are installed and loaded (See :doc:`setup`). 
  However, MIP should tell you if something is missing.

4. To install the perl module dependencies - use cpanm:

  .. code-block:: console
    
    cpanm <dependency>
    $ cpanm YAML

.. _Miniconda: http://conda.pydata.org/miniconda.html