# Fog::OpenNebula
[![OpenNebula](https://img.shields.io/badge/one-5.8.5-blue.svg?style=flat-square)](https://opennebula.org)

## Installation

Gem is not yet on rubygems, so you'll need to build the gem from source code

```bash
 cd fog-opennebula
 gem build fog-opennebula.gemspec
 gem install fog-opennebula-0.0.1.gem
```

## Description

Interaction with [OpenNebula](http://www.opennebula.org) is done via the [Ruby OpenNebula Cloud API](http://docs.opennebula.org/stable/integration/system_interfaces/ruby.html).

**Note:** This provider is under construction! This means everything that is provided should work without problems, but there are many features not available yet. Please contribute!

## Requirements

- Ruby version 2.0.x and higher
- gems
	- fog-core (>= 0)
	- fog-json (>= 0)
	- fog-xml (>= 0)
	- minitest (>= 0, development)
	- minitest-stub-const (>= 0, development)
	- mocha (~> 1.1.0, development)
	- opennebula (>= 0)
	- rake (>= 0, development)
	- shindo (~> 0.3.4, development)
	- simplecov (>= 0, development)
	- xmlrpc (>= 0)
- Working OpenNebula instance with XML-RPC and oneadmin user credentials
- OpenNebula (tested version is 5.4.x)

## Usage

General proceeding:

- connect to OpenNebula xml-rpc
- create new vm object
- fetch a template/flavor from OpenNebula (this template should be predefined)
- assign the flavor/template to the vm
- change the attributes of this flavor/template (name, cpu, memory, nics....)
- save/instantiate the vm

```ruby
require 'fog/opennebula'

con = Fog::Compute.new(
  provider: 'OpenNebula',
  opennebula_username: 'oneadmin',
  opennebula_password: 'password',
  opennebula_endpoint: 'http://localhost:2633/RPC2'
)

# list all vms
con.servers

# list all OpenNebula templates
con.flavors

# get template with id 0
con.flavors.get 0

# list all Virtual Networks
con.networks
con.networks.get 0

# get all usergroups
con.groups

# create a new vm object (creates the object, the vm is not instantiated yet)
newvm = con.servers.new

# set the flavor of the vm
newvm.flavor = con.flavors.get 0

# set the name of the vm
newvm.name = "FooBarVM"

# set the groupid of the vm
newvm.gid = 0

# set cores and memory (MB)
newvm.flavor.vcpu = 2
newvm.flavor.memory = 256

# create a new network interface attached to the network with id 0 and virtio as driver/model
network = con.networks.get(0)
nic = con.interfaces.new({ :vnet => network, :model => "virtio"})

# Attach the new nic to our vm
newvm.flavor.nic = [ nic ]

# instantiate the new vm
newvm.save
```

## Additional Resources

- [Fog website](http://fog.io)
- [Fog repo](https://github.com/fog/fog)
- [Ruby OpenNebula Cloud API](http://docs.opennebula.org/stable/integration/system_interfaces/ruby.html)
