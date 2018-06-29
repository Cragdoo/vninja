---
author: cmohn
comments: true
date: 2011-07-07 08:10:55+00:00
layout: post
slug: setting-up-automated-esxi-deployments
title: Setting Up Automated ESXi Deployments
url: /virtualization/setting-up-automated-esxi-deployments/
wordpress_id: 724
categories:
- Howto
- Virtualization
- VMware
tags:
- automation
- ESXi
- fun
- Ops
- PowerCLI
- Powershell
- realworld
- Virtualization
- VMware
---

Automating ESXi installs was made much easier after the release of vSphere 4.1 where the Scripted Install feature was added, and by using [VMware Auto Deploy](http://labs.vmware.com/flings/vmware-auto-deploy) from [VMware Labs](http://labs.vmware.com/). VMware Auto Deploy requires that you have vCenter and Host Profiles in your environment, and that again requires that you have Enterprise Plus licenses in your environment.

It is, however, possible to deploy ESXi in an automated fashion completely without vCenter and Host Profiles! By using a combination of a PXE based installation and [PowerCLI](http://communities.vmware.com/community/vmtn/vsphere/automationtools/powercli) for automating the setup of ESXi after the initial deployment. As this setup has been put together for a specific work project, my PowerCLI script also copies a VM template to the deployed ESXi host as well as the [vMA](http://communities.vmware.com/community/vmtn/vsphere/automationtools/vima) for administrative tasks. There is one caveat with regards to this setup though, and that is that the free version of ESXi only allows PowerCLI in read-only mode. This means that you will either need to get licenses for the ESXi install, or use trial licenses. With the price drop from VMware on the [Remote Office / Branch Office (ROBO)](http://store.vmware.com/store/vmware/en_US/DisplayProductDetailsPage/productID.169075300) licenses, we're looking at using that licensing model for our fleet of vessels.


#### Overview


The "complete package" consists of the following components:



	
  * **Deployment VM**
This is a custom VM, built to provide DHCP and PXE services to do the actual ESXi installation

	
  * **Powershell + PowerCLI**
Scripts that configure the ESXi host, post installation, and copy your initial VMs to the new host


Our current process looks like this:

	
  1. Connect physical host to deployment laptop via ethernet

	
  2. Start deployment VM on deployment laptop

	
  3. When deployment VM is finished booting, start physical host

	
  4. Physical host boots of network and PXE and installs ESXi

	
  5. When ESXi installation finishes, run PowerCLI script against host

	
  6. Disconnect deployment laptop and physical host, and connect physical host to vessel network

	
  7. Connect vSphere Client to ESXi install and start server VM




#### Deployment VM


Since our deployment scenario might be a bit out of the ordinary, we have the deployment VM set up on VMware Workstation or VMware Player on a laptop. The reason for this is that we need a mobile deployment model as the locations we are deploying this on are not static. Not only are they mobile, they are actually floating around on rather large oceans. That's right, we're deploying ESXi hosts on our vessels world wide!



##### Basic VM Setup


The basic setup is a standard Windows Server 2008 R2 with IIS installed. We will not be using any of the DHCP or other networking features included in Server 2008. In our environment it's configured with a static ip of **172.16.200.1
**



##### DHCP + PXE Setup


For DHCP and PXE services, we are using [Tftpd32](http://tftpd32.jounin.net/) a free and open source application that provides us with all the required services for deployment eg. both DHCP and PXE.



###### Kickstart Script


Our very basic kickstart script - _ks.cfg_ - looks like this
  [cc lang="kixtart" escaped="true" width="95%"]
vmaccepteula
rootpw password
autopart --firstdisk --overwritevmfs
install url http://172.16.200.1/ESXi
network --bootproto=dhcp --device=vmnic0
reboot
[/cc]

Basically this sets the root password, automatically deletes all partitions and sets up a new vmfs, tells the installer that it will find the installation files via http on the server and sets the networking configuration to DHCP. This will of course need tweaking in your environment, but this should at least get you started with building your own. More details on the ks.cfg bootstrap commands can be found in the [ESX and vCenter Server Installation Guide](http://www.vmware.com/pdf/vsphere4/r41/vsp_41_esx_vc_installation_guide.pdf)


#### PowerCLI Configuration Script


[cc lang="powershell" escaped="true" width="95%" height="700"]
########################################################
#
# Created by Christian Mohn
# for Seatrans AS
#
# No warranty suggested or implied
#
########################################################`

#connect to VirtualCenter or ESX host
#
function Register-VMX {
param($entityName = $null,$dsNames = $null,$template = $false,$ignore = $null,$checkNFS = $false,$whatif=$false)

function Get-Usage{
Write-Host "Parameters incorrect" -ForegroundColor red
Write-Host "Register-VMX -entityName -dsNames [,...]"
Write-Host "entityName : a cluster-, datacenter or ESX hostname (not together with -dsNames)"
Write-Host "dsNames : one or more datastorename names (not together with -entityName)"
Write-Host "ignore : names of folders that shouldn't be checked"
Write-Host "template : register guests ($false)or templates ($true) - default : $false"
Write-Host "checkNFS : include NFS datastores - default : $false"
Write-Host "whatif : when $true will only list and not execute - default : $false"
}

if(($entityName -ne $null -and $dsNames -ne $null) -or ($entityName -eq $null -and $dsNames -eq $null)){
Get-Usage
break
}

if($dsNames -eq $null){
switch((Get-Inventory -Name $entityName).GetType().Name.Replace("Wrapper","")){
"Cluster"{
$dsNames = Get-Cluster -Name $entityName | Get-VMHost | Get-Datastore | where {$_.Type -eq "VMFS" -or $checkNFS} | % {$_.Name}
}
"Datacenter"{
$dsNames = Get-Datacenter -Name $entityName | Get-Datastore | where {$_.Type -eq "VMFS" -or $checkNFS} | % {$_.Name}
}
"VMHost"{
$dsNames = Get-VMHost -Name $entityName | Get-Datastore | where {$_.Type -eq "VMFS" -or $checkNFS} | % {$_.Name}
}
Default{
Get-Usage
exit
}
}
}
else{
$dsNames = Get-Datastore -Name $dsNames | where {$_.Type -eq "VMFS" -or $checkNFS} | Select -Unique | % {$_.Name}
}

$dsNames = $dsNames | Sort-Object
$pattern = "*.vmx"
if($template){
$pattern = "*.vmtx"
}

foreach($dsName in $dsNames){
Write-Host "Checking " -NoNewline; Write-Host -ForegroundColor red -BackgroundColor yellow $dsName
$ds = Get-Datastore $dsName | Select -Unique | Get-View
$dsBrowser = Get-View $ds.Browser
$dc = Get-View $ds.Parent
while($dc.MoRef.Type -ne "Datacenter"){
$dc = Get-View $dc.Parent
}
$tgtfolder = Get-View $dc.VmFolder
$esx = Get-View $ds.Host[0].Key
$pool = Get-View (Get-View $esx.Parent).ResourcePool

$vms = @()
foreach($vmImpl in $ds.Vm){
$vm = Get-View $vmImpl
$vms += $vm.Config.Files.VmPathName
}
$datastorepath = "[" + $ds.Name + "]"

$searchspec = New-Object VMware.Vim.HostDatastoreBrowserSearchSpec
$searchspec.MatchPattern = $pattern

$taskMoRef = $dsBrowser.SearchDatastoreSubFolders_Task($datastorePath, $searchSpec)

$task = Get-View $taskMoRef
while ("running","queued" -contains $task.Info.State){
$task.UpdateViewData("Info.State")
}
$task.UpdateViewData("Info.Result")
foreach ($folder in $task.Info.Result){
if(!($ignore -and (&{$res = $false; $folder.FolderPath.Split("]")[1].Trim(" /").Split("/") | %{$res = $res -or ($ignore -contains $_)}; $res}))){
$found = $FALSE
if($folder.file -ne $null){
foreach($vmx in $vms){
if(($folder.FolderPath + $folder.File[0].Path) -eq $vmx){
$found = $TRUE
}
}
if (-not $found){
if($folder.FolderPath[-1] -ne "/"){$folder.FolderPath += "/"}
$vmx = $folder.FolderPath + $folder.File[0].Path
if($template){
$params = @($vmx,$null,$true,$null,$esx.MoRef)
}
else{
$params = @($vmx,$null,$false,$pool.MoRef,$null)
}
if(!$whatif){
$taskMoRef = $tgtfolder.GetType().GetMethod("RegisterVM_Task").Invoke($tgtfolder, $params)
Write-Host "`t" $vmx "registered"
}
else{
Write-Host "`t" $vmx "registered" -NoNewline; Write-Host -ForegroundColor blue -BackgroundColor white " ==> What If"
}
}
}
}
}
Write-Host "Done"
}
}

# Register-VMX -entityName "MyDatacenter"
# Register-VMX -dsNames "datastore1","datastore2"
# Register-VMX -dsNames "datastore1","datastore2" -template:$true
# Register-VMX -entityName "MyDatacenter" -ignore "SomeFolder"
# Register-VMX -dsNames "datastore3","datastore4" -ignore "SomeFolder" -checkNFS:$true
# Register-VMX -entityName "MyDatacenter" -whatif:$true

if ($args[0] -eq $null)
#
{$hostName = Read-Host "Enter ESX Host Name or IP"}
#
else
#
{$hostName = $args[0]}
#
#
#connect to selected Virtualcenter or host server
#
Connect-VIServer $hostName

# Set Datastore Name
$dsName = "datastore1"
$ds = Get-Datastore -Name $dsName
New-PSDrive -Name $dsName -Root \ -PSProvider VimDatastore -Datastore $ds

# Copy and Register VM from local drive to ESXi Host
Copy-DatastoreItem C:\VMs\MyTestVM\* datastore1:\MyTestVM\ -Force
Register-VMX -dsNames $dsName

# Import vMA ovf
Import-VApp -Source c:\VMs\vMA\vMA-4.1.0.0-268837.ovf -VMHost $hostName -Datastore $ds

# Lets configure the host

########################################################
# Host Configuration #
########################################################

#Disable IPv6
Get-VMHostNetworkAdapter | where { $_.PortGroupName -eq "Service Console 1" } | Set-VMHostNetworkAdapter -IPv6Enabled $false

# Configure networking
# Not finished

# Configure NTP Server
Add-VMHostNtpServer -VMHost $hostName -NtpServer "0.vmware.pool.ntp.org"
Add-VMHostNtpServer -VMHost $hostName -NtpServer "1.vmware.pool.ntp.org"
Add-VMHostNtpServer -VMHost $hostName -NtpServer "2.vmware.pool.ntp.org"

# Set VM Start Policy
$vmstartpolicy = Get-VMStartPolicy -VM MyTestVM
Set-VMStartPolicy -StartPolicy $vmstartpolicy -StartAction PowerOn
[/cc]

This PowerCLI script is not 100% finished yet, the networking part remains to be automated to provide correct configuration based on which vessel we are deploying to, but in general it's pretty much good to go.

Of course, there are loads of ways to extend and improve this process, but for now this suits our needs very well. I'm sure I'll need to revise it once vSphere.next is out and ready for deployment. 

_Using a model like this, combined with some interesting usage patterns for vMA you can create an automated ESXi deployment scenario that let's you deploy, [patch](http://vninja.net/virtualization/using-vma-as-a-local-vsphere-patch-repository/) and manage your [remote vSphere infrastructure](http://vninja.net/news/7-expert-tips-for-managing-your-remote-vsphere-infrastructure/) in a pretty streamlined fashion._
