---
layout: post
title:  "Introducing Jujuna!"
date:   2018-11-23 13:15:46 +0100
author: "<a href='https://github.com/matuskosut' target='_blank'>Matúš Košút</a>"
categories: jujuna introduction update
---

At [HUNT Cloud](https://www.ntnu.edu/huntgenes/hunt-cloud) we run our scientific services based on OpenStack orchestrated by Juju. Such cloud deployments rely on a large set of collaborative softwares, and upgrades can sometimes cause considerable pain. We are therefore introducing [Jujuna](https://github.com/huntdatacenter/jujuna) - a tool to simplify the validation of OpenStack upgrades with Juju.

<!--more-->

New to [Juju](https://jujucharms.com/)? Juju is a cool controller and agent based tool from Canonical to easily deploy and manage applications (called Charms) on different clouds and environments (see [how it works](https://jujucharms.com/how-it-works) for more details).

## What does Jujuna do?

Jujuna validates OpenStack upgrades from your specific bundle definition to one of the OpenStack releases supported by the OpenStack charms. First, Jujuna automates the deployment of OpenStack in a testing model with Juju based on your bundle. Next, it automates the upgrade of both the charms revisions and the actual OpenStack release, including rolling upgrades of HA services (when possible) based on the steps from the [OpenStack Charms Deployment Guide](https://docs.openstack.org/project-deploy-guide/charm-deployment-guide/latest/app-upgrade-openstack.html). Then, it validates the status of the OpenStack services during and after the deployment. Finally, it can clean up the deployment.

## Why did we write Jujuna?

[The OpenStack charmers](https://github.com/openstack-charmers) write great code that is robustly tested. Still, our local bundle configurations and specific upgrade scenarios do from time to time cause issues under upgrades. We noticed that most of these were related to upgrades between specific bundle versions and charm revision. Therefore, we wrote Jujuna to allow for systematic testing of such upgrades. The main aim was to increase our confidence in the upgrades prior to production changes.

## How does Jujuna work?

Jujuna is mainly written in Python (3.5+) and it is utilizing [python-libjuju](https://github.com/juju/python-libjuju), the native python library for communication with Juju controllers. Jujuna test suites are written in Yaml similar to Juju bundles.

<img src="/assets/img/jujuna_upgrade.png" alt="image" style="padding:30px;">

Jujuna provides four main functions that allow us to assemble various pipelines and test multiple scenarios: deploy, upgrade, test, and cleanup. These functions allow us to properly test software upgrades, from simple tests up to multistage upgrades.

{% highlight shell %}
jujuna
  ├── __init__.py
  ├── __main__.py
  ├── brokers
  │   ├── __init__.py
  │   ├── api.py
  │   ├── file.py
  │   ├── mount.py
  │   ├── network.py
  │   ├── package.py
  │   ├── process.py
  │   ├── service.py
  │   └── user.py
  ├── clean.py
  ├── deploy.py
  ├── exporters
  │   ├── __init__.py
  │   ├── file.py
  │   ├── mount.py
  │   ├── network.py
  │   ├── package.py
  │   ├── process.py
  │   ├── service.py
  │   └── user.py
  ├── helper.py
  ├── settings.py
  ├── tests.py
  └── upgrade.py
{% endhighlight %}



## What are the main use cases fo Jujuna?

We use Jujuna for three main purposes at [HUNT Cloud](https://www.ntnu.edu/huntgenes/hunt-cloud), all to test desired deployments and service upgrades of OpenStack. We utilize a dedicated stack of test hardware, with very similar configuration to our production site. We deploy the OpenStack Juju bundle with all the applications that we have in production, although at a smaller scale.


### Case 1: Continuous integration

Test of configuration changed as a part of bundle repository CI. Everytime the Juju bundle is changed it is automatically deployed and tested. All the results are pushed back to our CI. Passing result from pipeline approves the change.

### Case 2: Revision updates

New charm revisions are released more often than the services. Release time also depends on channels that charm developers use. You can regularly run Jujuna to test new or nightly releases from edge channel of charm revisions.

### Case 3: Service upgrades

Test before upgrade. Whenever there is need to upgrade production services, you can easily deploy your test stack, upgrade required services, and run your testing suite. We find both upgrade processes and testing useful to identify potential issues.


## Community

All the contributions are welcome.
Follow us on GitHub (https://github.com/huntdatacenter)

## Acknowledgements

HUNT Cloud team: Matus Kosut, Sandor Zeestraten, Tom-Erik Røberg, Oddgeir Lingaas Holmen.

HUNT Cloud is affiliated to the HUNT Research Centre, Department of Public Health and Nursing, Faculty of Medicine and Health Sciences, [NTNU](https://www.ntnu.edu/), Norway.
