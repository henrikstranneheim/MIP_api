Installation
==============

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