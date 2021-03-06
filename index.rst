.. title:: Nutanix Certified Services

.. toctree::
  :maxdepth: 2
  :caption: Deployment Services
  :name: _lab1
  :hidden:

  lab1/lab1
  lab2/lab2
  volumes_deploy/volumes_deploy
  files_deploy/files_deploy
  objects_deploy/objects_deploy

.. .. toctree::
..   :maxdepth: 1
..   :caption: Data Protection Services
..   :name: _lab4
..   :hidden:
..
..   lab4/lab4
..
.. .. toctree::
..   :maxdepth: 1
..   :caption: Migration Services
..   :name: _lab3
..   :hidden:
..
..   move/move
..
.. .. toctree::
..   :maxdepth: 1
..   :caption: Fit Check Services
..   :name: _lab5
..   :hidden:
..
..   lab5/lab5

.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/command_reference
  appendix/windows_tools_vm
  appendix/linux_tools_vm
  appendix/glossary

.. _getting_started:

++++++++++++++
Introduction
++++++++++++++

Welcome to the Nutanix Certified Services Training Labs. These labs accompanies an instructor-led session that introduces Nutanix concepts and reinforces knowledge learned in classes.

You will be taken through a Nutanix cluster Foundation which sets up nodes and associated resources(networking, storage, etc)

You will explore Prism Element and become familiar with its features and navigation. You will use Prism to perform basic cluster administration tasks, including storage and networking.

At the end of the bootcamp, attendees should understand the Core concepts and technologies that make up the Nutanix Enterprise Cloud stack and Karbon offering and should be well prepared for a hosted or onsite proof-of-concept (POC) engagement.

.. note::

	Estimated time to complete this lab is 90 minutes

What's New
+++++++++++

- Workshop updated for the following software versions:
    - AOS & PC 5.11.2.x

- Optional Lab Updates:

Lab Agenda
+++++++++++

NCS Labs:

- Foundation
- Using Crashcart Tool
- 1-Click upgrade
- Install Prism Central
- Creating As Built Guide
- AHV Networking
- VM Snapshot Management
- Tools - NCC, As Built Document Generator, Log Collector
- Volumes
- Files
- Objects

Initial Setup
++++++++++++++

- Take note of the *Passwords* being used.
- Log into your virtual desktops (connection info below)

Environment Details
++++++++++++++++++++

Nutanix Workshops are intended to be run in the Nutanix Hosted POC environment. Your cluster will be provisioned with all necessary images, networks, and VMs required to complete the exercises.

Networking
..........

Hosted POC clusters follow a standard naming convention:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**21**.\ *XYZ*\ .0
- **Cluster IP** - 10.**21**.\ *XYZ*\ .37

If provisioned from the marketing pool:

- **Cluster Name** - MKT\ *XYZ*
- **Subnet** - 10.**20**.\ *XYZ*\ .0
- **Cluster IP** - 10.**20**.\ *XYZ*\ .37

For example:

- **Cluster Name** - POC055
- **Subnet** - 10.21.55.0
- **Cluster IP** - 10.21.55.37

Throughout the Workshop there are multiple instances where you will need to substitute *XYZ* with the correct octet for your subnet, for example:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

Each cluster is configured with 2 VLANs which can be used for VMs:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

Credentials
...........

.. note::

  The *<Cluster Password>* is unique to each cluster and will be provided by the leader of the Workshop.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

Each cluster has a dedicated domain controller VM, **DC**, responsible for providing AD services for the **NTNXLAB.local** domain. The domain is populated with the following Users and Groups:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   .. * - SSP Admins
   ..   - adminuser01-adminuser25
   ..   - nutanix/4u
   .. * - SSP Developers
   ..   - devuser01-devuser25
   ..   - nutanix/4u
   .. * - SSP Consumers
   ..   - consumer01-consumer25
   ..   - nutanix/4u
   .. * - SSP Operators
   ..   - operator01-operator25
   ..   - nutanix/4u
   .. * - SSP Custom
   ..   - custom01-custom25
   ..   - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

Access Instructions
+++++++++++++++++++

The Nutanix Hosted POC environment can be accessed a number of different ways:

Lab Access User Credentials
...........................

PHX Based Clusters:
**Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), **Password:** *<Provided by Instructor>*

RTP Based Clusters:
**Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), **Password:** *<Provided by Instructor>*


Employee Pulse Secure VPN
..........................

Download the client:

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

For RTP:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com

Frame VDI
.........

Login to: https://frame.nutanix.com/x/labs

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Parallels VDI
.................

PHX Based Clusters Login to: https://xld-uswest1.nutanix.com

RTP Based Clusters Login to: https://xld-useast1.nutanix.com

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Nutanix Version Info
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.17.x.x
- **PC Version** - pc.2020.7
