# SQL2014 Cluster Calm Blueprint

## Description

This blueprint deploys a SQL2014 Failover Cluster. 

This Blueprint also contains 1 lifecycle action:
**Create_SQL_DB** - This action creates a SQL Database on the SQL2014 Failover Cluster. With this, Administrators do not need to login to the SQL VMs and launch SSMC to create a DB. It can be done directly via Calm.

It follows the following steps:
1. Creates 2 VMs.
2. Joins Domain.
3. Enables WSman-CredSSP.
4. Starts the iSCSI Initiator Service.
5. Calls Nutanix RESTapi to Create Volume Groups and 3 Disks (1 Witness, 1 Data, 1 Logs).
6. Calls Nutanix RESTapi to allow VM IQNs to connect to the Volume Groups.
7. VMs connect to the iSCSI Target, initializes and format the disks.
8. Installs Failover Clustering Roles on VMs.
9. Configures Windows Server Failover Cluster on VMs.
10. Installs SQL2014 in Failover Cluster Mode on VM1.
11. Installs SQL2014 in AddNode Mode on VM2.


## Requirements
- Nutanix Data Services IP must be set for VMs to access Volume Groups.
- **pre-sysprepped image** (VM which has Windows Server Installed, sysprepped and shutdown) must be used. Blueprint is tested with W2K16.
- SQL2014 Installation Media has to be uploaded into the Nutanix Image Service.
- AHV with IPAM Networks must be used.

## Credentials
WINDOWS - Credential that will be used to login to VM Image on first boot.
Domain_Admin - Credential that will be used to join Domain, create WSFC, Install SQL.
Nutanix_Prism_Creds - Credentials that will be used to call the Nutanix RESTapi.
SQL_SVC_Account - Credentials that will be used for SQL2014 Failover Cluster Service Accounts. 

## Variables
MSSQL1_VMName : VM Name for SQL VM 1.
MSSQL1_VM_Public_IP : VM Public (Cluster) IP for SQL VM 1.
MSSQL1_VM_Private_IP : VM Private (Heartbeat) IP for SQL VM1.
MSSQL2_VMName : VM Name for SQL VM 2.
MSSQL2_VM_Public_IP : VM Public (Cluster) IP for SQL VM 2.
MSSQL2_VM_Private_IP : VM Private (Heartbeat) IP for SQL VM2.
WSFC_Cluster_Name : Windows Server Failover Cluster DNS Name.
WSFC_Cluster_IP : Windows Server Failover Cluster IP Address.
SQL_ClusterName : SQL2014 Failover Cluster DNS Name.
SQL_Cluster_VirtualIP : SQL2014 Failover Cluster IP Address.
Prism_Element_Cluster_IP : Nutanix Prism **Element** Cluster IP address.
Prism_Element_DataServices_IP : Nutanix Prism **Element** Data Services IP.
Nutanix_Container_Name : Nutanix Storage Container Name that exists on the cluster, where Volume Groups will be created on.
Volume_GroupName : Name of Volume Group that will be created on Nutanix.
DB_Disk_Size_GB : Size of Data Disk, in GB.
Log_Disk_Size_GB : Size of Logs Disk, in GB.
DomainName : Domain which VMs will be joined to.
