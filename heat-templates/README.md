# Heat Templates with Cloudstrap Usage Examples

This directory contains the following example heat templates:

* 'sensu-masterless.yaml`: Deploys a Sensu server configured through masterless puppet.
* 'sensu-master-agent.yaml`: Deploys a Puppet master, Sensu server and Sensu client (the latter receive their configuration from the Puppet master).

You can instantiate these stacks by issuing a

  `heat stack-create -f sensu-masterless.yaml -P deploy_key=$(cat <my_deploy_key>) -P key_name=<my_nova_keypair_name> -P public_net_id=dc4d2dfb-f8d2-461c-9f16-636edbf99a0f my-masterless-stack`

for the masterless puppet setup, and
  
  `heat stack-create -f sensu-master-agent.yaml -P deploy_key=$(cat <my_deploy_key>) -P key_name=<my_nova_keypair_name> -P public_net_id=dc4d2dfb-f8d2-461c-9f16-636edbf99a0f my-master-agent-stack`

for the Puppet master/agent setup. Substitute the path to your deploy key for `<my_deploy_key>` and your Nova keypair name for `<my_nova_keypair_name>`.

If you would like to use Syseleven's
[scloud](https://github.com/syseleven/python-cloudutils) utility, you can
instatiate these stacks using the scloud configuration provided along with the
heat templates:

  `scloud --config-file ~/.scloud.conf --config-file sensu-masterless.conf`
  `scloud --config-file ~/.scloud.conf --config-file sensu-master-agent.conf`

This assumes you configured sensible defaults for the `key_name`, `deploy_key`
and `public_net_id` parameters in ~/.scloud.conf.
