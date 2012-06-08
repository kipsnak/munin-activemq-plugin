##Munin ActiveMQ plugin

Display queues status graphs in Munin.

## Requirements

### Perl modules

`WWW::Mechanize`

`XML::Simple`

## Install

### Debian

1. download `queue_` script to `/usr/share/munin/plugins/queue_` as root

` wget -P /usr/share/munin/plugins/ https://raw.github.com/kipsnak/munin-activemq-plugin/master/queue_ `

2. Test if your system has a required modules by the following command:

	/usr/share/munin/plugins/queue_ autoconf

as result you should have yes, else you can install them:

_System_:

	apt-get install libwww-mechanize-perl

	apt-get install libxml-simple-perl

_by user_:

	perl -MCPAN -e "install WWW::Mechanize"

	perl -MCPAN -e "install XML::Simple"


you can also use `force install`:

	perl -MCPAN -e "force install XML::Simple"


! You might have a lot of dependencies to install.

=> TODO: use another perl lib with less dependencies

3. Create a symlink to this script:

	cd /etc/munin/plugins/

	ln -s /usr/share/munin/plugins/queue_ /etc/munin/plugins/queue_<name.of.my.queue>

if you want display multi-graph for the same ActiveMQ queue try this:

	ln -s /usr/share/munin/plugins/queue_ /etc/munin/plugins/queue_<some.string>_<name.of.my.queue>

4. Configure the plugin environment variables:

In your Munin-node, edit the file `munin-node` in _/etc/munin/plugin-conf.d/_ and add the following lines:

	[queue_*]
		env.host <my.activemq_host>
		env.port <my.activemq.port>
		env.category activemq_queues
		env.type ({list.of.component})

here an exemple:
	[queue_*]
		env.host 127.0.0.1
		env.port 8161
		env.category activemq_queues
		env.type ('size','enqueueCount')

As default the plugin will display only 'size' component.

Nota: change `[queue_*]` to `[queue_<some.string>_*]` if you choose to display multi graph.

5. Test your config:
	munin-run /etc/munin/plugins/queue_<name.of.my.queue>

6. Restart your Munin-node:
	/etc/init.d/munin-node restart
