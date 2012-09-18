<properties umbracoNaviHide="0" pageTitle="Create a Virtual Machine Running Windows Server 2008 R2" metaKeywords="Windows Azure cloud services, cloud service, configure cloud service" metaDescription="Windows Tutorials." linkid="manage-windows-how-to-guide-storage-accounts" urlDisplayName="How to: storage accounts" headerExpose="" footerExpose="" disqusComments="1" />

<div chunk="../chunks/article-left-menu.md" />

# Create a Virtual Machine Running Windows Server 2008 R2 #

It's easy to create a virtual machine that is running the Windows Server operating system when you use the Image Gallery in the Windows Azure Management Portal. This tutorial will teach you how to create a virtual machine running Windows Server in the cloud that you can then access and customize. You don't need prior experience with Windows Azure to use this tutorial. 

You will learn:

- [What a virtual machine is in Windows Azure] []
- [How to use the Management Portal to create a custom virtual machine running Windows Server] []
- [How to log on to the virtual machine after you create it] []
- [How to attach a data disk to the new virtual machine] []
- [How to set up communication with the virtual machine] []
- [Next steps] []

<div chunk="../../Shared/Chunks/create-account-and-vms-note.md" />

<h2><a id="virtualmachine"> </a><span class="short-header">What is a virtual machine?</span>What a virtual machine is in Windows Azure</h2>

A virtual machine in Windows Azure is a server in the cloud that you can control and manage. After you create a virtual machine in Windows Azure, you can delete and re-create it whenever you need to, and you can access the virtual machine just like any other server. You use virtual hard disk (VHD) files to create a virtual machine. You can use the following types of VHDs to create a virtual machine:

- **Image** - An image is a VHD that you use as a template to create a new virtual machine. An image is a template because it doesn’t have specific settings like a running virtual machine, such as the computer name and user account settings. If you create a virtual machine using an image, an operating system disk is automatically created for the new virtual machine.
- **Disk** - A disk is a VHD that you can boot and mount as a running version of an operating system. A disk is a version of an image that you can run. Any VHD that is attached to virtualized hardware and that is running as part of a service is a disk. After an image is provisioned, it becomes a disk. A disk is always created when you use an image to create a virtual machine.

You can use the following options to create a virtual machine from an image:

- Create a virtual machine by using an image from the Image Gallery of the Windows Azure Management Portal.
- Create and upload a VHD file that contains an image to Windows Azure, and then create a virtual machine using the image. For more information about how to create and upload a custom image, see [Creating and Uploading a Virtual Hard Disk that Contains the Windows Server Operating System](/en-us/manage/windows/common-tasks/upload-a-vhd/).

<h2><a id="custommachine"> </a><span class="short-header">Create the virtual machine</span>How to use the Management Portal to create a custom virtual machine running Windows Server</h2>

You use the **From Gallery** feature to create a custom virtual machine in the Management Portal. When you create this virtual machine, you can define the size of the virtual machine, the connected resources, the DNS name, and the network connectivity if needed.


1. Sign in to the Windows Azure Management Portal.
2. On the command bar, click **New**.

	![Create new virtual machine] (../../../ITPro/Windows/media/create.png)

3. Click **Virtual Machine**, and then click **From Gallery**.
	
	![Create virtual machine from gallery] (../../../ITPro/Windows/media/createnew.png)

	The **VM OS Selection** dialog box appears. You can now select an image from the Image Gallery.

	![Select the OS image] (../../../ITPro/Windows/media/imageselectionwindows.png)

4. Click **Platform Images**, select the **Windows Server 2008 R2 SP1** image, and then click the arrow to continue.

	The **VM Configuration** dialog box appears.

	![Specify the details of the machine] (../../../ITPro/Windows/media/imagedefinewindows.png)

5. In **Virtual Machine Name**, type the name that you want to use for the virtual machine. For this virtual machine, type **MyTestVM1**.

6. In **New Password**, type a password for the Administrator account on the virtual machine. For this virtual machine, type **MyPassword1**. In **Confirm Password**, retype the password.

7. In **Size**, select the size of the virtual machine. The size that you select depends on the number of cores that are needed for your application. For this virtual machine, select **Extra Small**.

8. Click the arrow to continue.

	The **VM Mode** dialog box appears.

	![Specify the details of the machine] (../../../ITPro/Windows/media/imagestandalonewindows.png)

9. You can connect virtual machines together under a cloud service to provide robust applications, but for this tutorial, you are creating a single virtual machine. So, select **Standalone Virtual Machine**.

10. A virtual machine that you create is contained in a cloud service. In **DNS Name**, type a name for the cloud service that is created for the virtual machine. The entry can contain from 3-24 lowercase letters and numbers. This value becomes part of the URI that is used to contact the cloud service that the virtual machine belongs to. For this virtual machine, type **MyService1**.

11. Select the storage account for the VHD file. For this tutorial, select **Use Automatically Generated Storage Account**.

12. In **Region/Affinity Group/Virtual Network**, select **West US** as the location of the virtual machine.

13. Click the arrow to continue.

	The **VM Options** dialog box appears.

	![Specify the connection options of the machine] (../../../ITPro/Windows/media/imageoptionswindows.png)

14. The options on this page apply only if you are connecting this virtual machine to other virtual machines or if you are adding the virtual machine to a virtual network. For this virtual machine, you are not creating an availability set or connecting to a virtual network. Click the check mark to create the virtual machine.
    
	Windows Azure creates the virtual machine and configures the operating system settings. After Windows Azure creates the virtual machine, it is listed as **Running** in the Windows Azure Management Portal.

	![Successful virtual machine creation] (../../../ITPro/Windows/media/vmsuccesswindows.png)

<h2><a id="logon"> </a><span class="short-header">Log on to the virtual machine</span>How to log on to the virtual machine after you create it</h2>

You can log on to the virtual machine that you created to manage both its settings and the applications that are running on it.

1. Sign in to the [Windows Azure Management Portal](http://www.windowsazure.com).

2. Click **Virtual Machines**, and then select the **MyTestVM1** virtual machine.

3. On the command bar, click **Connect**.

	![Connect to the virtual machine] (../../../ITPro/Windows/media/connectwindows.png)

4. Click **Open** to use the remote desktop protocol file that was automatically created for the virtual machine.

	![Use the remote desktop protcol file] (../../../ITPro/Windows/media/connectrdp.png)

5. Click **Connect**.

	![Continue with connecting] (../../../ITPro/Windows/media/connectpublisher.png)

6. In the password box, type **MyPassword1**, and then click **OK**.
	
	![Enter the password] (../../../ITPro/Windows/media/connectcreds.png)

7. Click **Yes** to verify the identity of the virtual machine.

	![Verify the identity of the machine] (../../../ITPro/Windows/media/connectverify.png)

	You can now work with the virtual machine just like you would a server in your office.

<h2><a id="attachdisk"> </a><span class="short-header">Attach a data disk</span>How to attach a data disk to the new virtual machine</h2>

Your application might need to store data. To set this up, attach a data disk to the virtual machine. The easiest way to do this is to attach an empty data disk to the virtual machine.

1. Sign in to the Windows Azure Management Portal.

1. Click **Virtual Machines**, and then select the **MyTestVM1** virtual machine.

2. On the command bar, click **Attach**, and then click **Attach Empty Disk**.

	![Attach empty disk] (../../../ITPro/Windows/media/attachdiskwindows.png)

	The **Attach Empty Disk** dialog box appears.

	![Attach empty disk] (../../../ITPro/Windows/media/attachnewdiskwindows.png)

3. The **Virtual Machine Name**, **Storage Location**, and **File Name** are already defined for you. All you have to do is enter the size that you want for the disk. Type **5** in the **Size** field.

	**Note:** All disks are created from a VHD file in Windows Azure storage. You can provide a name for the VHD file that is added to storage, but Windows Azure generates the name of the disk automatically.

4. Click the check mark to attach the data disk to the virtual machine.

5. Click the name of the virtual machine to display the dashboard; this lets you verify that the data disk was successfully attached to the virtual machine.

	The number of disks is now 2 for the virtual machine. The disk that you attached is listed in the **Disks** table.

	![Attach empty disk] (../../../ITPro/Windows/media/attachemptysuccess.png)

After you attach the data disk to the virtual machine, the disk is offline and not initialized. You have to log on to the virtual machine and initialize the disk before you can use it to store data.

1. Connect to the virtual machine by using the steps listed in **Log on to the virtual machine**.

2. After you log on to the virtual machine, open **Server Manager**. In the left pane, expand **Storage**, and then click **Disk Management**.

	![Initialize the disk in Server Manager] (../../../ITPro/Windows/media/servermanager.png)

3. Right-click **Disk 2**, and then click **Initialize Disk**.

	![Start initialization] (../../../ITPro/Windows/media/initializedisk.png)

4. Click **OK** to start the initialization process.

5. Right-click the space allocation area for Disk 2, click **New Simple Volume**, and then finish the wizard with the default values.

	![Create the volume] (../../../ITPro/Windows/media/initializediskvolume.png)

	The disk is now online and ready to use with a new drive letter.

	![Initialization success] (../media/initializesuccess.png)

<h2><a id="endpoints"> </a><span class="short-header">Set up communication</span>How to set up communication with the virtual machine</h2>

All virtual machines that you create in Windows Azure can automatically communicate with other virtual machines in the same cloud service or virtual network. However, you need to add an endpoint to a virtual machine for other resources on the Internet or other virtual networks to communicate with it. You can associate specific ports and a protocol to endpoints.

1. Sign in to the Windows Azure Management Portal.

2. Click **Virtual Machines**, and then select the **MyTestVM1** virtual machine.

3. Click **Endpoints**.

	![Add endponts] (../../../ITPro/Windows/media/endpointswindows.png)

4. For this tutorial, you will add an endpoint for communicating with the virtual machine using the TCP protocol. Click **Add Endpoint**.

	![Endpoints] (../../../ITPro/Windows/media/addendpointstart.png)

	The **Add Endpoint** dialog box appears.

	![Add TCP endpont] (../../../ITPro/Windows/media/addendpointwindows.png)

5. Accept the default selection of **Add Endpoint**, and then click the arrow to continue.

	The **New Endpoint Details** dialog box appears.

	![Define the endpont] (../../../ITPro/Windows/media/endpointtcpwindows.png)

6. In the Name field, type **MyTCPEndpoint1**.

7. In the **Public Port** and **Private Port** fields, type **80**. These port numbers can be different. The public port is the entry point for communication from outside Windows Azure. The Windows Azure load balancer uses the public port. You can use the private port and firewall rules on the virtual machine to redirect traffic in a way that is appropriate for your application.

8. Click the check mark to create the endpoint.

	You will now see the endpoint listed on the **Endpoints** page.

	![Endpont successfully created] (../../../ITPro/Windows/media/endpointwindowsnew.png)

<h2><a id="nextsteps"> </a><span class="short-header">Next steps</span>Next steps</h2>

- [How to connect virtual machines in a cloud service] [] - To ensure availability of your application and to balance the load of traffic to your application you must connect virtual machines together under a cloud service.
- [Load balance virtual machines] [] - After you connect virtual machines under a cloud service, you can define how the traffic load is balanced for the application.
- [Manage the availability of virtual machines] [] - By using availability sets, you can make sure that your application is always available.
- [Add a virtual machine to a virtual network] [] - You can add your new virtual machine to a virtual network.

[How to connect virtual machines in a cloud service]:../../../ITPro/Windows/HowTo/howto-connect-vm-cloud-service.md
[Load balance virtual machines]:../../../ITPro/Windows/CommonTasks/LoadBalancingVirtualMachines.md
[Manage the availability of virtual machines]:../../../ITPro/Windows/CommonTasks/manage-vm-availability.md
[Add a virtual machine to a virtual network]:../../../ITPro/Services/networking/Tutorial3_AddVMachineToVNet.md



[What a virtual machine is in Windows Azure]: #virtualmachine
[How to use the Management Portal to create a custom virtual machine running Windows Server]: #custommachine
[How to log on to the virtual machine after you create it]: #logon
[How to attach a data disk to the new virtual machine]: #attachdisk
[How to set up communication with the virtual machine]: #endpoints
[Next steps]: #nextsteps