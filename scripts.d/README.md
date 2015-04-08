Description
===========

This directory may contain user supplied scripts to be run after bootstrapping.
The script ``scripts.d/additional`` will be run automatically if it exists
and is executable. It may expect the following environment variables to be
present:

* ``config_dir``          The directory this repository has been checked out to.
* ``scripts_dir``         The directory the Syseleven provided bootstrap-scripts repository has been checked out to.
* ``sys11_config_dir``    The directory Syseleven provided default configuration has been checked out to.
* ``scripts_branch``      The branch used for the bootstrap-scripts directory.
* ``config_repo``         The URL pointing to this repository.
* ``config_branch``       The branch used for this repository.
* ``repodeploy_repo``     The URL pointing to Syseleven's puppet-repodeploy repository.
* ``repodeploy_branch``   The branch used for puppet-repodeploy.
* ``sys11_config_repo``   The URL pointing to Syseleven's default configuration repository (sys11-config).
* ``sys11_config_branch`` The branch used for Syseleven's default configuration repository.

Additionally, ``${scripts_dir}/bin`` may be expected to be part of the shell's
PATH.
