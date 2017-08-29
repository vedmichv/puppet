# This is repo for final task 
## Final task
 1. Deploy 2 machines. 1 master/server other agents
 2. Install master and agents and connect agents to puppet master
 3. Create production and staging environment for puppet
 4. Create fews modules, one of them to install zabbix server, second for zabbix agent.
 5. Deploy zabbix server by puppet. 
 6. Write the second module, the module should install zabbix agent and connect it to zabbix server.

## Requirements

 1. Module should be able to setup correct set of repositories including signing keys
 2. Module should be able to make decision where it runs and understand when should be applied configuration for agent nodes and where for master node
 3. Puppet module should contain at least one custom fact for example `is_puppetmaster = true|false`
 4. Puppet module should contain erb tepmlates one or more
 5. Puppet module should be verified with puppet-lint and contains metadata.json
 5. Special Task 1: All static data should be saved in HIERA
 6. Special Task 2: Use Role and Profile aprouch to set up Zabbix server and agent
 7. Special Task 3: setup module management with r10k (https://github.com/puppetlabs/r10k) r10k basic setup should be done with puppet
 8. Special Task 4: Encrypt password for access to Zabbix DB
## Defenition of done

 1. You have at least 2 VMs and can demostrate them, one is a puppet master and one is agent
 2. Your manifest can be applied locally on clear box and if you need predefines please ad them in documentation.
 3. Your code is available for review as pull request to this repository
 4. Vagrant file should be available in this repo
 
### Dead Line 04.09.2017 9AM MSQ time zone