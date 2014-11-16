# Ansible for Junos OS

This is an exmple ansible configuration demonstrating how to use ansible to generate and apply Junos configuration to the EX, MX, QFabric, QFX and SRX Juniper platforms.

It is assumed that you have an understanding Ansible and how it's structure works. If that is not the cane then please read through the [Ansible Documentation](http://www.ansible.com/docs).

The configuration generated by this example is not the complete configuration that can be applied to these platforms. It is expected that you will modify and extend the configuration to meet your own needs.

There are many wapys to use ansible to generate and apply configuration to devices. This example is heavily documented to provide the reader with an explanation for the design choices made.

## Prerequists

To be able to use the ansible configuration you must have a copy of [Ansible](http://www.ansible.com/install) installed. In order to apply the configuration generated you must have a copy of the [Junos plugin](https://github.com/Juniper/ansible-junos-stdlib.git) for Ansible installed.

It is assummed you are using a package manager to install Ansible and so other Ansible dependencies are installed when you install Ansible.

## Conventions

With Ansible's flexibility some choices have to be made. For some there are no clear right or wrong answer. Where is decision is made then it is documented. Either in this README or within the file - depending on where it is most appropriate.

## Structure

### Hosts File Structure

The ansible hosts file is one of the most important files to get structuarlly correct. The structure used allows for global, location, function and device specific data.

For further details please see the comments in the hosts file.

### Group Variables

The group_vars directory contains information appropriate globally, per site and per platform. This is made possible by the structure of the hosts file.

### Host Variables

The host_vars files contains device specific information for each of the devices listed in the hosts file. There must be one file for each host defined the hosts file.

### Playbooks

Playbooks are structured hierarchically. To have (syntactic) symatry with the hosts file the top playbook is the all.pb.yaml file. That playbook includes a playbook for each of the platforms. 

Each platform playbook defines the roles to execute, merges the configuration fragments and then apply the configuration if the configuration changed.

This structure takes advantage of Junos' ability to parse Junos configuration even if the configuration is not in order. This feature makes creating Junos configuration very simple in Ansible. This feature is used to break the configuration up into functional blocks that makes sense to the designer.

For further details please see the comments in each of the playbooks.

### Roles

Each of the roles just generates the configuration. Each role generates one or more configuration fragment that is merged into one file. Roles are made up of a task and one or more template file.

For further details please see the comments in the roles/*/tasks/main.yaml files.

### Templates

Templates are kept as simple as possible. The emphasis is to keep the Jinja2 constructs to the basis features. Jinja2 is a power templating system. To avoid overloading the reader with too many new concepts the use of advanced Jinja2 operations is avoided as much as possible.

For further details about the Jinja2 templating operators please see the comments in each of the playbooks.

## Execution

To execute this example - and apply the configuration generated by this example - use the following Linux CLI command:

    > ansible-playbook -i hosts all.pb.yaml

That will cause ansible to run through all of the plays, generating and applying configuration for all the devices.

### Limiting scope of execution

Ansible supports executing against a subset of the hosts. This is achieved with the [code]--limit[/code] option. You can limit on either a hostname or a group name. For example to only generate and apply configuration to all EXs in site 1 use the command:

    > ansible-playbook -i hosts all.pb.yaml --limit site1_ex

To limit to a host supply the hostname, like this:

    > ansible-playbook -i hosts all.pb.yaml --limit leaf1.site1.example.com

You can also limit execution by supplying a platform specific playbook. For example to apply changes to all MXs just use the mx.p.yaml like so:

    > ansible-playbook -i hosts mx.p.yaml

You may also use the [code]--limit[/code] option with this playbook.

## Further Documentaiton

Please look through the rest of the files. All of them contain documentation relevant to their purpose.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
