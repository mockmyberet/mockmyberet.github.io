---
layout: post
title: Creating multiple local VMs in Hyper-V
categories:
  - Powershell Stuffs
tags:
  - Powershell
  - Hyper-V
  - Virtualization
  - Automation
---

This is a quick script I wrote today to quickly build some VMs for my local Hyper-V lab running in Windows 10.

```posh
#requires -Version 2.0 -Modules Hyper-V
function New-VMFromScratch
{
  param
  (
    [String]
    [Parameter(Mandatory,HelpMessage='Enter the name of the local VM.',ValueFromPipeline,ValueFromPipelineByPropertyName)]
    $VMName
  )
  begin
  {
    $VMName = $VMName.ToUpper()
  }
  process
  {
    $null = New-VHD -Path C:\Hyper-V\$VMName.vhdx -SizeBytes 40GB -Dynamic
    $null = New-VM -Name $VMName -MemoryStartupBytes 1GB -BootDevice CD -SwitchName LAN -Path C:\Hyper-V\ -Generation 2 -NoVHD
    $null = Get-VMDvdDrive -VMName $VMName |
    ForEach-Object -Process {
      Remove-VMDvdDrive -VMName $PSItem.VMName -ControllerNumber $PSItem.ControllerNumber -ControllerLocation $PSItem.ControllerLocation
    }
    $null = Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path C:\Hyper-V\ISOs\en_windows_server_2016_x64_dvd_9718492.iso
    $null = Add-VMHardDiskDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 0 -Path C:\Hyper-V\$VMName.vhdx
    $null = Set-VM -Name $VMName -ProcessorCount 2 -DynamicMemory -MemoryMinimumBytes 256MB -MemoryMaximumBytes 2gb -AutomaticStartAction Start -CheckpointType Disabled -AutomaticStopAction ShutDown
    $VMDVD = Get-VMDvdDrive -VMName $VMName
    $VMVHD = Get-VMHardDiskDrive -VMName $VMName
    $null = Set-VMFirmware -VMName $VMName -BootOrder $VMDVD, $VMVHD
    Get-VM -Name $VMName
  }
  end
  {
  }
}

```
