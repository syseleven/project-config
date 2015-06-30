Description
===========

This repository contains project specific configuration for Openstack instances
deployed using Syseleven's cloudstrap framework.

Structure
=========

puppet/sys11-topcis contains a list of configuration topics to be pulled in
from sys11-configs.

The remaining configuration resides in the puppet/hieradata subdirectory. This
directory contains four *type directories*, grouping configuration by
configuration type: nodes.d, nodetype.d, config.d and repos.d.

config.d and repos.d contain *topic directories* (e.g. sensu-server/ for
deploying a sensu server or ssh/ for rolling out SSH keys and configuring the
SSH server). For any given topic there may be a topic subdirectories in any of
config.d and repos.d. Both of these directories' contents override
configuration from their counterparts in the sys11-config repository.

nodes.d and nodetype.d contain node or node type specific configuration that is
pulled in on the basis of the ::fqdn and ::nodetype facts.

scripts.d may contain user-supplied scripts.

scripts.d/
----------

This directory contains bootstrap stages to be executed by initialize_instance.
Scripts are numbered to control the order they are executed in (think sysvinit
styles rc.d/ directories).  You may place additional bootstrapping scripts in
your project-config repository

For numbering your own scripts there are two rules: 

* Numbers must be written in three-digit format (e.g. '015' instead of '15')

* Multiples of 20, including '000' (e.g. '000', '020', '040') are reserved for
  Syseleven's use. Apart from that anything goes (just pick a number that will
  insert your own script between the desired Syseleven scripts.

Scripts in scripts.d may expect the following environment variables to be present:

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

puppet/sys11-topics
-------------------

This file contains contains a list of all configuration topics to be included
from the sys11-config repository. It may contain comment lines denoted by a
leading '#' and duplicate entries (any duplicate entries are filtered out by
the hiera.yaml generator). This file must contain all configuration that is
not fully duplicated in the project repository. A topic's mention in this file
will cause this topic's config.d and repos.d entry to be pulled in from
sys11-config.

puppet/hieradata/nodes.d
-------------------------

This directory contains node specific configuration. Its contents are added to
the hierarchy based on the following fact interpolation:

  "project-config/nodes.d/%{::fqdn}"

This means that all files in this directory must have the desired node's
fully-qualified domain name as their filename (and the extension .yaml, of
course).

At minimum node configuration files must contain a classes array containing the
classes to be deployed on the node in question. config.d and repos.d
configuration for these classes must either be pulled in from sys11-config
by adding the relevant topics to puppet/sys11-topics (see above) or by adding
it to puppet/hieradata/repos.d and puppet/hieradata/config.d in full (the
latter is not recommended). Apart from a classes array, node configuration may
contain any configuration parameters meant exclusively for a given node.
Configuration in nodes.d will take precedence over anything apart from
override.yaml (the latter is passed as a heat parameter at stack creation
time).

Each node to be managed by puppet must have at least one entry that matches its
::fqdn or ::nodetype fact, respectively in either puppet/hieradata/nodes.d or
puppet/hieradata/nodetypes.d (or both).


puppet/hieradata/nodetypes.d
----------------------------

This directory contains node type specific configuration. Its contents are
added to the hierarchy based on the following fact interpolation:

  "project-config/nodetypes.d/%{::nodetype}"

This means that all files in this directory must have a valid node type as
their filename (and the extension .yaml, of course). A node is assigned a
nodetype through the 'nodetype' metadata parameter. This parameter's value is
returned by the ::nodetype fact.

At minimum node type configuration files must contain a classes array
containing the classes to be deployed on nodes with the node type in question.
config.d and repos.d configuration for these classes must either be pulled in
from sys11-config by adding the relevant topics to puppet/sys11-topics (see
above) or by adding it to puppet/hieradata/repos.d and puppet/hieradata/config.d
in full (the latter is not recommended). Apart from a classes array, node type
configuration may contain any configuration parameters meant exclusively for
a given node type. Configuration in nodetypes.d will take precedence over
anything apart from nodes.d and override.yaml (the latter is passed as a heat
parameter at stack creation time).

Each node to be managed by puppet must have at least one entry that matches its
::fqdn or ::nodetype fact, respectively in either puppet/hieradata/nodes.d or
puppet/hieradata/nodetypes.d (or both).

puppet/hieradata/config.d
-------------------------

This directory's topic directories contain all configuration relevant to the
topic in question. This directory must not contain *classes* arrays or
puppet-repodeploy configuration. Configuration that is common to multiple
topics should go into a file named common.yaml in one of the topic directories
involved that is symlinked from the other topic directories.

*Note: This directory is an override for sys11-config. Hence only configuration
parameters with values differing from those in sys11-config needs to (and
should) be included here. Anything else not set explicitely will inherit the
default values from sys11-config.*

puppet/hieradata/repos.d
------------------------

This directory's topic directories contain puppet-repodeploy repository
configuration (i.e. the *repodeploy::repos hash*) specifiying all repositories
required to configure the topic configuration in question. This directory must
not contain any other configuration.

*Note: This directory is an override for sys11-config. Hence only configuration
parameters with values differing from those in sys11-config need to (and
should) be included here. Anything not set explicitely will inherit the default
values from sys11-config.*

heat-templates
--------------

This directory contains a variety of Heat templates suitable for use with
Cloudstrap.

Configuration Hierarchy
=======================

Configuration inheritance follows the following hierarchy (values set higher up
in the hierarchy override those set further down):

* Values set in override.yaml (This file's contents are passed as a heat parameter.)

* Node configuration in this repository's puppet/hieradata/nodes.d directory

* Node type configuration in this repository's puppet/hieradata/nodetypes.d directory

* Default configuration pulled in from sys11-config

* Configuration used for the masterless puppet bootstrapping stage (only
  relevant for the puppet master).

Naming Conventions for topic directories
========================================

This section contains naming conventions for the content of topic directories.

* Configuration files that should appear in Hiera's hierarchy in a given order
  (useful for config files that override each other's variables) should be
  named such that a directory listing shows them in that order. To that end
  they should be prefixed with a number and a dash, e.g. *00-overrides.yaml*
  (files with lower numbers will then appear first in the hierarchy, thus
  overriding those with lower numbers). In particular this applies to
  repository configurations in under repos.d which may need to be overriden
  temporarily until local fixes are merged upstream. To this end all permanent
  repository configuration files should start with '10-' to allow for easy
  override insertion.
