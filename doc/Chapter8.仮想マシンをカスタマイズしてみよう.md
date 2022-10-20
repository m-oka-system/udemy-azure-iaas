# Chapter8.仮想マシンをカスタマイズしてみよう

## 参考サイト

- [Azure の仮想マシンとIPアドレス](https://blogs.technet.microsoft.com/mskk-cloudos/2016/04/06/azure-ip/)

- [Azure における IP アドレスの種類と割り当て方法](https://docs.microsoft.com/ja-jp/azure/virtual-network/virtual-network-ip-addresses-overview-arm)

- [Azure Portal を使用して仮想マシンに複数の IP アドレスを割り当てる](https://learn.microsoft.com/ja-jp/azure/virtual-network/ip-services/virtual-network-multiple-ip-addresses-portal)

- [Azure の仮想マシンのサイズ](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/sizes)

- [Azure マネージド ディスクの概要](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/managed-disks-overview#a-nameimages-versus-snapshotsa%E3%82%A4%E3%83%A1%E3%83%BC%E3%82%B8%E3%81%A8%E3%82%B9%E3%83%8A%E3%83%83%E3%83%97%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88)

- [Managed Disks の価格](https://azure.microsoft.com/ja-jp/pricing/details/managed-disks/)

- [仮想マシンの複製方法のご紹介 (Part.1)](https://blogs.technet.microsoft.com/jpaztech/2018/09/21/arm-vm-replication-part-1/)

- [実行コマンドを使用して Windows VM でスクリプトを実行する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/run-command)

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
