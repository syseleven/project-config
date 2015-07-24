Description
===========

This directory's topic directories contain puppet-repodeploy repository
configuration (i.e. the *repodeploy::repos hash*) specifiying all repositories
required to configure the topic configuration in question. This directory must
not contain any other configuration.

File and directory names in this directory are free-form (every file with a
.yaml extension will be included), but it is recommended to organize
configuration by topic name for easier navigation by developers.

Example: `./graphite/10-syseleven.yaml`

    # Repositories to fetch puppet modules from.

    '/opt/puppet-modules/sys11graphite':
        revision: v6.0.0
        source: https://github.com/syseleven/puppet-sys11graphite.git
        provider: git

For more information see 
https://github.com/syseleven/puppet-repodeploy/blob/master/manifests/init.pp

*Note: This directory is an override for sys11-config. Hence only configuration
parameters with values differing from those in sys11-config need to (and
should) be included here. Anything not set explicitely will inherit the default
values from sys11-config.*
