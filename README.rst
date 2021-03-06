
==========
Ceilometer
==========

The ceilometer project aims to deliver a unique point of contact for billing
systems to acquire all of the measurements they need to establish customer
billing, across all current OpenStack core components with work underway to
support future OpenStack components.

Sample pillars
==============

Ceilometer API/controller node

.. code-block:: yaml

    ceilometer:
      server:
        enabled: true
        version: havana
        cluster: true
        secret: pwd
        bind:
          host: 127.0.0.1
          port: 8777
        identity:
          engine: keystone
          host: 127.0.0.1
          port: 35357
          tenant: service
          user: ceilometer
          password: pwd
        message_queue:
          engine: rabbitmq
          host: 127.0.0.1
          port: 5672
          user: openstack
          password: pwd
          virtual_host: '/openstack'
        database:
          engine: mongodb
          host: 127.0.0.1
          port: 27017
          name: ceilometer
          user: ceilometer
          password: pwd

Client-side RabbitMQ HA setup

.. code-block:: yaml

    ceilometer:
      server:
        ....
        message_queue:
          engine: rabbitmq
          members:
          - host: 127.0.0.1
          - host: 127.0.0.1
          - host: 127.0.0.1
          user: openstack
          password: pwd
          virtual_host: '/openstack'
       ....


Ceilometer Graphite publisher

.. code-block:: yaml

    ceilometer:
      server:
        enabled: true
        publisher:
          graphite:
            enabled: true
            host: 10.0.0.1
            port: 2003

Ceilometer compute agent

.. code-block:: yaml

    ceilometer:
      agent:
        enabled: true
        version: havana
        secret: pwd
        identity:
          engine: keystone
          host: 127.0.0.1
          port: 35357
          tenant: service
          user: ceilometer
          password: pwd
        message_queue:
          engine: rabbitmq
          host: 127.0.0.1
          port: 5672
          user: openstack
          password: pwd
          virtual_host: '/openstack'
          rabbit_ha_queues: true

Read more
=========

* https://wiki.openstack.org/wiki/Ceilometer
* http://docs.openstack.org/developer/ceilometer/install/manual.html
* http://docs.openstack.org/developer/ceilometer/
* https://fedoraproject.org/wiki/QA:Testcase_OpenStack_ceilometer_install
* https://github.com/spilgames/ceilometer_graphite_publisher
* http://engineering.spilgames.com/using-ceilometer-graphite/

Things to improve/consider
==========================

* Graphite publisher http://engineering.spilgames.com/using-ceilometer-graphite/
* Juno additions - Split Events/Meters and Alarms databases, Polling angets are
HA now, active/Activr Workload partitioning to central agents
* Kilo additions - Splint Events - Meters - Agents, notification agents are HA
now (everything is HA now), events - elastic search
* User notifier publisher vs rpc publisher (Juno+)
* Enable jittering (rendom delay) to polling. (Kilo+)
* Collect what you need - pipeline.yaml, tweak polling interval (Icehouse+)
* add more agents as load inceases (Juno+)
* Avoid open-ended queries - query on a time range
* Install api behind mod_wsgi, tweak wsgi daemon - threads and processes
* Set TTL - expire data to minimise database size
* Run Mongodb on separate node - use sharding and replica-sets

Deployment scenarios
--------------------

* Lambda design - use short term and long term databases in the same time
* Data segragation - separatem
* JSON files - Apache spark
* Fraud detection - proprietary alarming system
* Custom consumers - kafka - Apache Storm (kilo+)
* Debugging - Collecttions - Elastic serach - Kibana
* Noisy services - Multiple notification buses

Documentation and Bugs
============================

To learn how to deploy OpenStack Salt, consult the documentation available
online at:

    https://wiki.openstack.org/wiki/OpenStackSalt

In the unfortunate event that bugs are discovered, they should be reported to
the appropriate bug tracker. If you obtained the software from a 3rd party
operating system vendor, it is often wise to use their own bug tracker for
reporting problems. In all other cases use the master OpenStack bug tracker,
available at:

    http://bugs.launchpad.net/openstack-salt

Developers wishing to work on the OpenStack Salt project should always base
their work on the latest formulas code, available from the master GIT
repository at:

    https://git.openstack.org/cgit/openstack/salt-formula-ceilometer

Developers should also join the discussion on the IRC list, at:

    https://wiki.openstack.org/wiki/Meetings/openstack-salt

Documentation and Bugs
======================

To learn how to install and update salt-formulas, consult the documentation
available online at:

    http://salt-formulas.readthedocs.io/

In the unfortunate event that bugs are discovered, they should be reported to
the appropriate issue tracker. Use Github issue tracker for specific salt
formula:

    https://github.com/salt-formulas/salt-formula-ceilometer/issues

For feature requests, bug reports or blueprints affecting entire ecosystem,
use Launchpad salt-formulas project:

    https://launchpad.net/salt-formulas

You can also join salt-formulas-users team and subscribe to mailing list:

    https://launchpad.net/~salt-formulas-users

Developers wishing to work on the salt-formulas projects should always base
their work on master branch and submit pull request against specific formula.

    https://github.com/salt-formulas/salt-formula-ceilometer

Any questions or feedback is always welcome so feel free to join our IRC
channel:

    #salt-formulas @ irc.freenode.net
