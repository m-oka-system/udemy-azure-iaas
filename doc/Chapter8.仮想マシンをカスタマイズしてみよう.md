# Chapter8.仮想マシンをカスタマイズしてみよう

## 参考サイト

- [Azure の仮想マシンとIPアドレス](https://learn.microsoft.com/ja-jp/archive/blogs/mskk-cloudos/azure-ip)

- [パブリック IP アドレス](https://learn.microsoft.com/ja-jp/azure/virtual-network/ip-services/public-ip-addresses)

- [Azure Portal を使用して仮想マシンに複数の IP アドレスを割り当てる](https://learn.microsoft.com/ja-jp/azure/virtual-network/ip-services/virtual-network-multiple-ip-addresses-portal)

- [Azure の仮想マシンのサイズ](https://learn.microsoft.com/ja-jp/azure/virtual-machines/sizes)

- [Azure マネージド ディスクの概要](https://learn.microsoft.com/ja-jp/azure/virtual-machines/managed-disks-overview)

- [Managed Disks の価格](https://azure.microsoft.com/ja-jp/pricing/details/managed-disks/)

- [仮想マシンの複製方法のご紹介 (Part.1)](https://learn.microsoft.com/ja-jp/archive/blogs/jpaztech/arm-vm-replication-part-1)

- [実行コマンドを使用して Windows VM でスクリプトを実行する](https://learn.microsoft.com/ja-jp/azure/virtual-machines/windows/run-command)

## コマンド集

#### イメージから仮想マシンを作成する
```powershell
# Variables
$rgName = "w-iaas-rg"
$location = "Japan West"
$vnetName = "w-iaas-vnet"
$vmName = "w-iaas-vm01"
$vmSize = "Standard_B1ls"
$diskName = $vmName + "-os-disk"
$diskType = "Standard_LRS" #Premium_LRS
$diskSize = 32
$pipName = $vmName + "-pip"
$nicName = $vmName + "-nic"
$privateIP = "10.0.1.10"
$imageName = "w-iaas-win2022-ja-image"
$imageId = (Get-AzImage -ResourceGroupName $rgName -ImageName $imageName).Id
$adminUser = "cloudadmin"
$adminPassword = ConvertTo-SecureString "InputYourPassword!" -AsPlainText -Force
$osCreateOption = "FromImage"

# Create public ip address and network interface
$vnet = Get-AzVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
$pip = New-AzPublicIpAddress -ResourceGroupName $rgName -Location $location -Name $pipName -SKU Standard -AllocationMethod Static
$nic = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location -Name $nicName -SubnetId $vnet.Subnets[0].Id -PrivateIpAddress $privateIP -PublicIpAddressId $pip.Id

# Create user object
$cred = New-Object System.Management.Automation.PSCredential ($adminUser, $adminPassword)

# Create VM configuration
$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize
$vm = Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
$vm = Set-AzVMSourceImage -VM $vm -Id $imageId
$vm = Set-AzVMOSDisk -VM $vm -Name $diskName -DiskSizeInGB $diskSize -StorageAccountType $diskType -CreateOption $osCreateOption -Windows
$vm = Set-AzVMBootDiagnostic -VM $vm -Enable

# Create VM
New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm -Verbose
```
