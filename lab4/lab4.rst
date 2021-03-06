.. _lab4:

.. title:: Deployment Services

Data Protection Services
++++++++++++++++++++++++++++++++++++++++++++++

Local Snapshot and Recovery
------------------------------------------

For this lab you will need some test virtual machines. Follow the steps to create new VM.

#. Log into Prism

#. Locate Windows 2012 VM in Prism Element > VM

#. Right-click on the Windows 2012 VM and select clone (do not power on this template VM)

#. Name the clone as **Initials**-Win2012

#. To Install NGT:

   Click the Windows 2012 VM and choose **Manage Guest Tools**

   Select all options and click **Submit**

   .. figure:: images/ngt.png

#. From the System browse to the CDROM drive and run the installation with all defaults

#. Eject CDROM

#. Click on **+ Protection Domain** button > **Async DR**

#. Create a protection domain with a unique name by going through the wizard

#. Name the protection domain **Intials-PD-Prod**

#. Choose VMs to include in the protection domain

#. Choose the cloned VM you created in the last lab

#. Choose “Protect Selected Entities” click Next

#. Click “New Schedule” Setup a schedule for the protection domain

#. Choose **Repeat every 1 day**

#. Create Schedule

#. Click on **close**

#. Simulate a few days of snapshots by selecting the Async DR you created in table view

#. Select **Take Snapshot** and hit **save** and repeat a few times.

   - Hint: You may modify the VM so that you can snapshot different version of your VM

   - Under “Local Snapshots” for this Protection Domain you should see a few listed

We will now restore VM from Replication:

#. Select the Protection Domain containing the VM snapshot

#. Choose the Local Snapshot under **Local Snapshots** with the timestamp and click **Restore**

#. Choose all VMs or just certain VMs that you wish to restore

#. Create new entities:

   - Choose to create new entity to restore to a new VM. (Prefix: Nutanix-Clone-)
   - Look at VMs to see there are new VMs restore from your snapshots

Self Service Restore (SSR)
---------------------------

#. Console to your VM

#. Launch SSR Icon from the desktop

#. Login with your local administrator account

#. Notice you should see all snapshots available to you for that VM

#. Select a snapshot and choose to mount it as a drive letter

#. Windows File Explorer should see the snapshot as a local drive

#. Unmount and exit

Replicate to Remote Site & Recover Remote and Migrate/Activate
---------------------------------------------------------------

Follow these steps to create a Remote Site to replicate to.

#. From Data Protection Click **Remote Site** and select **Physical Cluster**

#. Give Remote Site a name **<Your Initials-REMOTE>**

#. Choose Capabilities **Disaster Recovery** this will allow Remote Recovery

#. Enter the IP address  of your partners cluster and choose **Add Site**

#. Choose Cluster to Cluster network mapping

#. Choose vStore A to VStore B.

.. note::

You must make a remote site from your partners cluster to your cluster and they should go from vStore B to vStore A

Modify your Data Protection Group
---------------------------------------------------------------

#. Select your Protection Domain and click **update**

#. Modify your Schedule and you will now see a remote site

#. Select to keep last 30 on Remote site

#. Then click **Create Schedule**

Take a few snapshots
---------------------------------------------------------------

#. Select your Protection Domain

#. Click **Take Snapshot**

#. Make sure the Remote site is selected and hit Save

#. Repeat step 2 & 3 a few times (at least 5 times)

#. The first replication should take the longest, click under “Replications” to watch the status


Restoring from Remote Snapshots
---------------------------------------------------------------

When replications are done, a full copy and its deltas are sent to the remote site.

To recover remote snapshot from local site A:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Select **Remote Snapshots**

#. Select a remote snapshot to bring back as a local snapshot for local recovery

Snapshot to Remote Site & use Migrate/Activate
---------------------------------------------------------------

Scenario #1
^^^^^^^^^^^^^

To move the VM from site A to Site B

#. From Site A, select your Protection Domain

#. Choose **Migrate** and notice all the VMs in that Protection Domain should be removed from Site A nd should now show in site B to be powered on (Fail-Over)

#. Feel free to continue work on the VM and make changes and repeat those steps 1&2 to migrate the Protection Domain back to Site A (Fail-Back)

Scenario #2
^^^^^^^^^^^^^^

When Site A has Failed and went down on its own and you want to bring it back online in Site B.

#. From Site B, select your Protection Domain

#. Choose **Activate**

#. This will bring the protection domain’s VMs online on remote site

   .. note::

     You may need to power on the VMs after activation of the Protection Domain

#. When Site A is considered back online the Migrate button should now be able to send the latest back to Site A

   .. note::

     Data Protection Best Practices:

     If you activate a PD because the primary site is down but the primary site comes back up after the failover, you can have a split-brain scenario. To resolve this situation, deactivate the PD on the former primary site. The following command is hidden from the nCLI because it deletes the VMs, but it resolves the split while keeping the existing snapshots:

     .. code-block:: bash

       ncli> pd deactivate_and_destroy_vms name=<protection_Domain_Name>

     Reference: `Rollback Steps Technote <https://portal.nutanix.com/page/documents/solutions/details?targetId=BP-2005-Data-Protection:top-failover-migrate-vs-activate.html>`_

     If we active the DR site while the Primary site is till Active, VMs will be registered on DR site as well.
     If both the sites are active, we need to destroy the VMs and PD on one of the site hence its recommended to reach out to `support <https://www.nutanix.com/support-services/product-support/support-phone-numbers>`_ before taking any action.

.. Snapshot to Remote Site
.. ---------------------------------------------------------------
..
.. Recover a snapshot at Remote Site B
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
..
.. #. From the site B look at “local snapshots”
.. #. Recover one of your snapshots in Site B
