# pfSense Setup & Configuration Guide

This guide documents the setup process for pfSense in a lab environment with **ECorp** and **Attack LAN** networks, DNS/DHCP configuration, and firewall rules. Each step corresponds to screenshots named `pf01`–`pf68` stored in `./images/`.


## Table of Contents
- [pfSense Setup \& Configuration Guide](#pfsense-setup--configuration-guide)
  - [Table of Contents](#table-of-contents)
  - [Accessing pfSense](#accessing-pfsense)
  - [Initial Setup Wizard](#initial-setup-wizard)
  - [Finalize Wizard](#finalize-wizard)
  - [Name the Interfaces](#name-the-interfaces)
  - [DNS Resolver](#dns-resolver)
  - [Networking Settings](#networking-settings)
  - [DHCP Leases \& Static IP](#dhcp-leases--static-ip)
  - [Create an Alias](#create-an-alias)
  - [Firewall Rules — ECorp](#firewall-rules--ecorp)
  - [Firewall Rules — Additional](#firewall-rules--additional)
  - [Firewall Rules — AttackLAN](#firewall-rules--attacklan)
  - [Internet Access Control for Kali](#internet-access-control-for-kali)
  - [Block WAN Subnets from LAN](#block-wan-subnets-from-lan)
  - [Reboot pfSense](#reboot-pfsense)


## Accessing pfSense

On your Windows 11 VM, open a browser and navigate to the pfSense IP (e.g., `https://10.0.1.1`).  
*You will see a certificate warning because pfSense uses a self-signed certificate.*

![pfSense Certificate Warning](/images/pf01.png)

Then you will be presented with a link to continue and a warning.  
*This is still due to the self-signed certificate.*

![pfSense Continue Warning](/images/pf02.png)

You will then be presented with a login page. The default credentials are:  
- **Username:** `admin`  
- **Password:** `pfsense`

![pfSense Login](/images/pf03.png)

## Initial Setup Wizard

Select **Next** to start the wizard.  
![Wizard Next 1](/images/pf04.png)

Select **Next** again.  
![Wizard Next 2](/images/pf05.png)

You can change the **Hostname** and **Domain** if you’d like. **Uncheck** **Override DNS**, then select **Next**.  
*Unchecking Override DNS ensures pfSense uses your chosen DNS settings, not those pushed by WAN.*  
![Hostname & Domain](/images/pf06.png)

Change the **Timezone** and click **Next**.  
![Timezone](/images/pf07.png)

Scroll to the bottom of the page and **uncheck** **Block RFC1918 Private Networks**, then select **Next**.  
*This allows private (RFC1918) networks to communicate on the WAN interface when appropriate for lab setups.*  
![RFC1918 Setting](/images/pf08.png)

Do not change any values and select **Next**.  
![Keep Defaults](/images/pf09.png)

Change the **Admin Password** and select **Next**.  
![Change Admin Password](/images/pf10.png)

## Finalize Wizard

Select **Reload**.  
![Reload](/images/pf11.png)

Then select **Finish**.  
![Finish](/images/pf12.png)

Select **Accept**.  
![Accept](/images/pf13.png)

Select **Close**.  
![Close](/images/pf14.png)

## Name the Interfaces

Go to **Interfaces** and select **LAN**.  
![Select LAN](/images/pf15.png)

Change the description to **ECorp** and select **Save**.  
![Rename LAN to ECorp](/images/pf16.png)

Select **Apply Changes**.  
![Apply Changes LAN](/images/pf17.png)

Change the name of the **OPT1** interface.  
![Select OPT1](/images/pf18.png)

Type **Attack LAN** and select **Save**.  
![Rename OPT1 Attack LAN](/images/pf19.png)

Apply changes.  
![Apply Changes OPT1](/images/pf20.png)

## DNS Resolver

Go to **Services** and select **DNS Resolver**.  
![Open DNS Resolver](/images/pf21.png)

Scroll down to the bottom of the page and check both **DHCP Registration** and **Static DHCP**.  
*This lets DNS automatically register DHCP hostnames, including static mappings.*  
![DHCP & Static DHCP](/images/pf22.png)

Scroll back up and select **Advanced Settings**.  
![DNS Advanced Settings](/images/pf23.png)

Under **Advanced Resolver Options**, enable **Prefetch Support** and **Prefetch DNS Key Support**.  
*Prefetch reduces latency by refreshing cached entries and DNSSEC keys proactively.*  
![Prefetch Options](/images/pf24.png)

Scroll down and select **Save**.  
![Save DNS Resolver](/images/pf25.png)

Select **Apply Changes**.  
![Apply DNS Changes](/images/pf26.png)

## Networking Settings

Now go to **System** → **Advanced**.  
![System Advanced](/images/pf27.png)

Then select **Networking**.  
![Networking Tab](/images/pf28.png)

Scroll down to **Network Interfaces** and enable **Hardware Checksum Offloading**. Then select **Save**.  
*This can improve performance on certain NICs; disable if you encounter NIC offload issues.*  
![Checksum Offloading](/images/pf29.png)

Select **OK**.  
![OK Networking Save](/images/pf30.png)

## DHCP Leases & Static IP

Login with the new password you set up.  
![Login New Password](/images/pf31.png)

Once logged in, go to **Status** and select **DHCP Leases**.  
![DHCP Leases](/images/pf32.png)

We can see the Windows 11 IP.  
![Windows 11 Lease](/images/pf33.png)

Select the **+** sign under **Actions**.  
![Add Static Mapping](/images/pf34.png)

Insert the IP address `10.0.1.2`.  
![Enter 10.0.1.2](/images/pf35.png)

Scroll down and select **Save**.  
![Save Static Lease](/images/pf36.png)

Apply the changes.  
![Apply DHCP Changes](/images/pf37.png)

On the Windows 11 VM, open **Command Prompt** and run:
```cmd
ipconfig /release
ipconfig /renew
```
![Release/Renew](/images/pf38.png)

As seen below, when running `ipconfig /renew`, it picks up the static IP we created: `10.0.1.2`.  
![Static 10.0.1.2 Confirmed](/images/pf39.png)

## Create an Alias

Create an alias by going to **Firewall** and selecting **Aliases**.  
![Open Aliases](/images/pf40.png)

Then select **Add**.  
![Add Alias](/images/pf41.png)

Make the changes seen below and **Save**.  
![Alias Details](/images/pf42.png)

Apply the changes.  
![Apply Alias Changes](/images/pf43.png)

## Firewall Rules — ECorp

Go back to the pfSense dashboard to configure **Firewall Rules**.  
![Firewall Rules Dashboard](/images/pf44.png)

Go to the **ECorp** tab and select **Add** rule at the bottom.  
![Add ECorp Rule](/images/pf45.png)

Make the changes seen below and **Save**. *This allows ECorp devices to communicate.*  
![ECorp Allow Rule](/images/pf46.png)

Apply changes.  
![Apply ECorp Rule](/images/pf47.png)

Add another rule at the bottom.  
![Add Second ECorp Rule](/images/pf48.png)

Make the changes seen below. *This rule allows communication between ECorp and AttackLAN.*  
![ECorp ↔ AttackLAN Rule](/images/pf49.png)

Apply changes.  
![Apply ECorp↔AttackLAN Rule](/images/pf50.png)

## Firewall Rules — Additional

Select another rule at the bottom.  
![Add Another Rule](/images/pf51.png)

Make the changes seen below and **Save**.  
![Additional Rule Details](/images/pf52.png)

Add one more rule at the bottom to **block anything not specified by earlier rules**.  
![Add Final Block Rule](/images/pf53.png)

Make the changes seen below.  
![Block-All Rule Details](/images/pf54.png)

Apply changes.  
![Apply Additional Rules](/images/pf55.png)

## Firewall Rules — AttackLAN

Go to the **AttackLAN** tab.  
Select **Add** a rule at the bottom.  
![Add AttackLAN Rule](/images/pf56.png)

Make the changes seen below and **Save**.  
![AttackLAN Rule Details](/images/pf57.png)

Apply changes.  
![Apply AttackLAN Rule](/images/pf58.png)

Select **Add** a rule at the bottom.  
![Add Second AttackLAN Rule](/images/pf59.png)

Add the rule below.  
![Second AttackLAN Rule Details](/images/pf60.png)

Apply changes.  
![Apply AttackLAN Rules](/images/pf61.png)


## Internet Access Control for Kali

*Note: If you ever need Kali Linux to have temporary internet access (e.g., to update or install tools), disable the blocking rule.*

To disable a rule, select the **edit** icon (pencil).  
![Edit Rule](/images/pf62.png)

Then select **Disable this rule**, **Save**, and **Apply Changes**.  
![Disable & Apply](/images/pf63.png)


## Block WAN Subnets from AttackLAN

Create another firewall rule at the **bottom of the LAN interface**.  
![Add Block WAN Rule](/images/pf64.png)

Make the following options and select **Save**. *This rule blocks any traffic from AttackLAN to WAN subnets.*  
![Block WAN Subnets Details](/images/pf65.png)

Then select **Apply changes**.  
![Apply Block WAN Rule](/images/pf66.png)


## Reboot pfSense

Go to **Diagnostics** and select **Reboot**.  
![Diagnostics Reboot](/images/pf67.png)

Choose **Normal Reboot** and select **Submit**.  
![Normal Reboot Submit](/images/pf68.png)


✅ **Setup Complete** — pfSense is now configured for **ECorp** and **Attack LAN** segmentation with DNS/DHCP services and firewall policies.
