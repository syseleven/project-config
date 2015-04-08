Description
===========

This directory's topic directories contain puppet-repodeploy repository
configuration (i.e. the *repodeploy::repos hash*) specifiying all repositories
required to configure the topic configuration in question. This directory must
not contain any other configuration.

*Note: This directory is an override for sys11-config. Hence only configuration
parameters with values differing from those in sys11-config need to (and
should) be included here. Anything not set explicitely will inherit the default
values from sys11-config.*
