
## Exercise - Estimate workload costs by using the Pricing calculator
For a basic web application hosted in your datacenter, you might run a configuration similar to the following.

An ASP.NET web application that runs on Windows. The web application provides information about product inventory and pricing. There are two virtual machines that are connected through a central load balancer. The web application connects to a SQL Server database that holds inventory and pricing information.

To migrate to Azure, you might:

Use Azure Virtual Machines instances, similar to the virtual machines used in your datacenter.
Use Azure Application Gateway for load balancing.
Use Azure SQL Database to hold inventory and pricing information.
Here's a diagram that shows the basic configuration:

A diagram showing a potential Azure solution for hosting an application.

![image](https://github.com/ZCHAnalytics/az-900/assets/146954022/580f9434-218e-4d44-9d95-feff4bb0b355)

In practice, you would define your requirements in greater detail. But here are some basic facts and requirements to get you started:

The application is used internally. It's not accessible to customers.
This application doesn't require a massive amount of computing power.
The virtual machines and the database run all the time (730 hours per month).
The network processes about 1 TB of data per month.
The database doesn't need to be configured for high-performance workloads and requires no more than 32 GB of storage.


![image](https://github.com/ZCHAnalytics/az-900/assets/146954022/a50d7bd0-45f5-43cb-852c-2aa259246dd4)


## Exercise - Compare workload costs using the TCO calculator
nvestigate whether there are any potential cost savings in moving your datacenter to the cloud over the next three years. You need to take into account all of the potentially hidden costs involved with operating on-premises and in the cloud.

et's say that:

You run two sets, or banks, of 50 virtual machines (VMs) in each bank.
The first bank of VMs runs Windows Server under Hyper-V virtualization.
The second bank of VMs runs Linux under VMware virtualization.
There's also a storage area network (SAN) with 60 TB of disk storage.
You consume an estimated 15 TB of outbound network bandwidth each month.
There are also a number of databases involved, but for now, you'll omit those details.

![image](https://github.com/ZCHAnalytics/az-900/assets/146954022/1fdba14a-c036-49c6-b4f8-64b62b390723)

![image](https://github.com/ZCHAnalytics/az-900/assets/146954022/43dee832-4471-482f-aed0-8124aa2cb605)

