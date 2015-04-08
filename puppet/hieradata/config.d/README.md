Description
===========

This directory's topic directories contain all configuration relevant to the
topic in question. This directory must not contain *classes* arrays or
puppet-repodeploy configuration. Configuration that is common to multiple
topics should go into a file named common.yaml in one of the topic directories
involved that is symlinked from the other topic directories.

*Note: This directory is an override for sys11-config. Hence only configuration
parameters with values differing from those in sys11-config need to (and
should) be included here. Anything else not set explicitely will inherit the
default values from sys11-config.*
