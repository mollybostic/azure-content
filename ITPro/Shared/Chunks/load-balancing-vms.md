#Load Balancing Virtual Machines#

To use this feature and other new Windows Azure capabilities, sign up for the [free preview](https://account.windowsazure.com/PreviewFeatures). 

External communication with virtual machines occurs through endpoints. These endpoints are used for diffierent purposes, such as load-balanced traffic or for direct Virtual Machine connectivity, like RDP or SSH. You define endpoints that are associated to specific ports and are assigned a specific communication protocol. An endpoint can be assigned a protocol of TCP or UDP (the TCP protocol includes HTTP and HTTPS traffic). Each endpoint defined for a virtual machine is assigned a public and private port for communication. The private port is defined for setting up communication rules on the virtual machine and the public port is used by Windows Azure to communicate with the virtual machine from external resources.

If you configure it, Windows Azure will provide round-robin load balancing of network traffic to publicly defined ports of a cloud service. When your cloud service contains instances of web roles or worker roles, you enable this load balancing by setting the number of instances running in the service to greater than two and by defining a public endpoint in the service definition. For Virtual Machines, you can set up load balancing by creating new virtual machines, connecting them together under a cloud service, and then adding load-balanced endpoints to the machines.

A load-balanced endpoint is a specific TCP or UDP endpoint that is used by all virtual machines contained in a cloud service. The following image shows a load-balanced endpoint that is shared among three virtual machines and uses a public and private port of 80.

![Load-balanced endpoints][Load-balanced endpoint]

This task includes the following steps:

- [Step 1: Create the first virtual machine] []
- [Step 2: Add an endpoint to the first virtual machine] []
- [Step 3: Add virtual machines to the cloud service] []
- [Step 4: Set up load balancing of the virtual machines] []
- [Step 5: (Optional) Define load-balancing probes] []

## <a id="firstmachine"> </a>Step 1: Create the first virtual machine ##

To create load-balanced virtual machines contained in a cloud service, you must create the first virtual machine and then connect additional virtual machines to this first machine. When you create this machine, a cloud service is created for you. The cloud service that is created to contain this virtual machine is not shown in the Management Portal until you connect additional virtual machines to the first machine. You can create the first virtual machine by using one of the following methods:

- **Quick Create** - You can quickly create a virtual machine by choosing an image from the Image Gallery and providing a name for the machine and an administrator password. For more information, see [How to quickly create a virtual machine] [].
- **From Gallery** - You can create a virtual machine by providing advanced settings, such as size, connected resources, DNS name, and network connection. For more information, see [How to create a custom virtual machine] [].

## <a id="addendpoint"> </a>Step 2: Add an endpoint to the first virtual machine ##

All virtual machines that you create in Windows Azure can automatically communicate using a private network channel with other virtual machines in the same cloud service or virtual network. However, you need to add an endpoint to a machine for other resources on the Internet or other virtual networks to communicate with it.

To set up load balancing for virtual machines, you add an endpoint to the first virtual machine that you create and then you assign that endpoint to be used on all other virtual machines that you connect to the first machine. For more information about creating an endpoint, see [How to set up communication with a virtual machine] [].

## <a id="addmachines"> </a>Step 3: Add virtual machines to the cloud service ##

When you create a virtual machine, a cloud service is automatically created to contain the machine. You can connect multiple virtual machines under the same cloud service to enable the machines to communicate with each other, to balance the load of network traffic to the machines, and to maintain high availability of the machines. For more information about connecting virtual machines, see [How to connect virtual machines in a cloud service] []. For more information about managing the availability of your application, see [Manage the availability of virtual machines] [].

## <a id="loadbalance"> </a>Step 4: Set up load balancing of the virtual machines ##

After you connect additional virtual machines to the first machine and you create an endpoint on the first machine, you can assign that endpoint to be used by the new machines for load balancing.

**To set up a load-balanced endpoint**

1. If you have not already done so, sign in to the Windows Azure Management Portal.

2. Click **Virtual Machines**, and then select one of the virtual machines that you connected to the first virtual machine.

	![Select a virtual machine][Select virtual machine]

3. Click **Endpoints**.

	![Select Endpoints][Select endpoints]

4. Click **Add Endpoint**.

	![Add Endpoint][Add endpoints]

5. Select **Load balance traffic on an existing endpoint**, choose the endpoint that you added to the first virtual machine, and then click the arrow to continue.

	![Add Load-Balanced Endpoint][Add lb endpoint]

6. In **Name**, enter a name for the endpoint.

7. In **Private Port**, type the port number that you want the virtual machine to use. You can use the private port and firewall rules on the virtual machine to redirect traffic in a way that is appropriate for your application. The public port is the same as the public port defined for the endpoint on the first virtual machine. The private port can be the same as the public port. For example, for an HTTP endpoint, you will likely want to use port 80 as the public port and the private port for all machines.

	![Define Load-Balanced Endpoint][Define endpoint]

8. Click the check mark to create the load-balanced endpoint. You will see **Yes** in the Load Balanced column of the Endpoints page for both virtual machines.

	![Load-Balanced Endpoint Success][Endpoint success]

9. Complete steps 2 through 8 for each virtual machine in the cloud service.

## <a id="lbprobes"> </a>Step 5: (Optional) Define load-balancing probes ##

A virtual machine must be in a healthy state to receive network traffic. You can optionally define your own method for determining the health of the virtual machine by adding a load-balancing probe to the load-balanced endpoint. Windows Azure probes for a response from the virtual machine every 15 seconds and takes a machine out of the rotation if no response is received after two probes. You must use PowerShell to define probes on the load balancer. For more information about using Windows Azure cmdlets, see [Getting Started with Windows Azure PowerShell] [].

To change the configuration of an existing virtual machine using PowerShell, you must get the object that represents the virtual machine, modify the configuration, and then update the object to save the changes. The following example uses the Get-AzureVM cmdlet to retrieve the virtual machine object, pipes the object to the Set-AzureEndpoint cmdlet to change the load-balanced endpoint settings, and then pipes the changes to the Update-AzureVM cmdlet:

	Get-AzureVM -ServiceName "MyService" -Name "MyTestVM2" | Set-AzureEndpoint -LBSetName "MyLBSet" –Name MyTestEndpoint2 –Protocol tcp –LocalPort 80 -ProbePort 80 -ProbeProtocol http -ProbePath "/" | Update-AzureVM

To run the cmdlets that are listed in the previous example, you must know the following information:

- **Service name** – The name of the cloud service in which the virtual machine is contained.
- **Name** – The name of the virtual machine to which the endpoint is attached.
- **Load balancing set name** – The name of the load balancing set to which the endpoint belongs. When you create a load-balanced endpoint, a load-balancing set is automatically created for you to contain the load-balanced endpoints. When you create load-balanced endpoints in the Management Portal, the load-balanced set name is the same as the first endpoint defined for the set.
- **Endpoint name** – The name of the endpoint to which you are adding the load-balancing probe.
- **Protocol** – The protocol that is used for communication with the endpoint. The TCP and UDP protocols are used for defining endpoints. The TCP protocol supports HTTP and HTTPS.
- **Local port** – The number of the port that is used for load balancing network traffic.
- **Probe port** – The port to which the load-balancing probe is associated.
- **Probe protocol** – the protocol that the probe is expecting.
- **Probe path** – The path that defines the action of the web server. This option is only needed if http traffic is expected on the endpoint. You can define the path to be “/”, or you can specify a page or application to run to provide the health status. If you specify “/”, the web server returns a status of 200, any other status will keep the virtual machine out of the load-balancing rotation. The URL that is configured for the load-balancing probe receives a GET request from Windows Azure without passing host headers or authentication of any kind. If the probe path you specify returns a 401 ACCESS DENIED then Windows Azure will not add the virtual machine to the rotation. It is important to configure a URL that can respond anonymously. If you specify a page or program to define health status, the resource must return 200 to enable the virtual machine to be included in the load-balancing rotation.

You can get information about endpoints that have been defined for a virtual machine by using the Get-AzureVM cmdlet and the Get-AzureEndpoint cmdlet.

	Get-AzureVM -ServiceName "MyTestVM1" -Name "MyTestVM2" | Get-AzureEndpoint

The previous command produces the following results:

![List Endpoint][List endpoint]

[Step 1: Create the first virtual machine]: #firstmachine
[Step 2: Add an endpoint to the first virtual machine]: #addendpoint
[Step 3: Add virtual machines to the cloud service]: #addmachines
[Step 4: Set up load balancing of the virtual machines]: #loadbalance
[Step 5: (Optional) Define load-balancing probes]: #lbprobes

[Load-balanced endpoint]:../media/loadbalancing.png
[Select virtual machine]:../media/selectvm.png
[Select endpoints]:../media/endpoints.png
[Add endpoints]:../media/addendpoint.png
[Add lb endpoint]:../media/addloadbalanceendpoint.png
[Define endpoint]:../media/endpointloadbalance.png
[Endpoint success]:../media/loadbalancedendpointsuccess.png
[List endpoint]:../media/listendpoints.png

[How to quickly create a virtual machine]:../../Windows/HowTo/howto-quick-create-vm.md
[Manage the availability of virtual machines]:../../Windows/CommonTasks/manage-vm-availability.md
[How to create a custom virtual machine]:../../Windows/HowTo/howto-custom-create-vm.md
[How to set up communication with a virtual machine]:../../Windows/HowTo/howto-setup-endpoints-vm.md
[How to connect virtual machines in a cloud service]:../../Windows/HowTo/howto-connect-vm-cloud-service.md
[Getting Started with Windows Azure PowerShell]:http://msdn.microsoft.com/en-us/library/jj156055.aspx