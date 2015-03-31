# Example heat-template for sys11 openstack 

this example deploys a puppet-master and a single server
connected to this puppet master.
This tempalte also includes security-groups which alllow 
to connect to your puppet-master via ssh and to your example 
node via HTTP(s).

Start this stack using

heat stack-create -f exampleStack.yaml -e exampleStackEnvironment.yaml -P deploy_key="$(cat ~/.ssh/cloud_deploykey)" $stackname

