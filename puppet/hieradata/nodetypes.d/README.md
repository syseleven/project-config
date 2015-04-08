Description
===========

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
