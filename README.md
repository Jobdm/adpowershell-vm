<h1>Active Directory with Powershell - Creating Virtual Machines </h1>
This section of the lab goes through the creation and configuration of our virtual machines. <br />

<h2>Environments and Technologies Used</h2>

- QEMU/KVM with Virt-Manager (Kernal Virtual Machine)

<h2>Operating Systems Used </h2>

- Linux Mint (Host OS)
    - Desktop environment: KDE Plasma 5.27
 
- Windows Server 2022 (Guest OS)
- Windows 10 Pro (Guest OS)

<h2>Required Files</h2>

- [Windows Server 2022 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)
- [Windows 10 Multi-Edition ISO](https://www.microsoft.com/en-us/software-download/windows10iso)
- [Virtio-win-0.1.266 ISO](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.266-1/)

<h2>Virtual Machine - Domain Controller</h2>

<details>
 <summary><b>- Configuration</b></summary>
 <p align="center">
<b>We need to create and tinker around with our virtual machine before we install Windows Server 2022 onto it.</b>
</p>
<p>In Virt-manager right-click `QEMU/KVM` -> `New`.</p>
<p align="center">
<img src="https://i.imgur.com/coVg9TZ.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>Select `Local install media` -> `forward`, then `browse local`. Select the ISO.</p>
<p align="center">
<img src="https://i.imgur.com/0HqRQR4.png" height="50%" width="50%" alt="enter role name"/>
<img src="https://i.imgur.com/eje7mg8.png" height="50%" width="50%" alt="enter role name"/>
</p>
<p>Go through the rest of the steps. Give your vm a name to help identify it later. Check `Customize configuration before install` then click `forward`.</p>
<p align="center">
<img src="https://i.imgur.com/bgUVeTi.png" height="80%" width="80%" alt="enter role name"/>
</p>
<br />
<p>We begin customization by going into 'Overview' and editing code in the 'XML' tab. These changes will make it so the VM uses less resources.</p>
<p align="center">
<img src="https://i.imgur.com/wLgzbcH.png" height="80%" width="80%" alt="enter role name"/>
</p>

<p>In 'CPU' we check `Manually set CPU topology` and set socket to `1`. We can adjust the cores and threads however we want, depending on our resources.</p>
<p align="center">
<img src="https://i.imgur.com/y4ZtQxL.png" height="80%" width="80%" alt="enter role name"/>
</p>

<p>Here we configure our virtual hard drive and and virtual network interface card.</p>
<p align="center">
<img src="https://i.imgur.com/KWGD0hR.png" height="100%" width="100%" alt="enter role name"/>
</p>
<br />
<p>We need to add some new virtual hardware: CDROM drive and Network interface card.</p>
<ul>
    <li>`Add Storage` -> `CDROM device` and click `manage`, then select the virtio-win iso. This will load the drivers for the VM.</li>
    <li>`Add Network` -> `Bridge device`. For 'device name' we need to enter the interface name of our bridge connection. It will be for the internal network.</li>
</ul>

<p align="center">
<img src="https://i.imgur.com/j3soy8U.png" height="40%" width="40%" alt="enter role name"/>
<img src="https://i.imgur.com/yOY7Qg9.png" height="40%" width="40%" alt="enter role name"/>
</p>
<hr />

<p align="center"><b>After double-checking our configuration we can begin installation.</b></p>
<p align="center">
<img src="https://i.imgur.com/togx8fm.png" height="80%" width="80%" alt="enter role name"/>
</p>
<br />
</details>    

<details>
 <summary><b>- OS Installation</b></summary>
 <p align="center">
 <b>Now we can install our operating system!</b>
 </p>

<p align="center">
<img src="https://i.imgur.com/I7Z53UZ.png" height="65%" width="65%" alt="end-user experience"/>
</p>

<p>Going through the windows install process...</p>
	<ul>
        <li>Select `Windows Server 2022 Standard Evaluation (Desktop Experience)`</li>
	    <li>Select `Custom Installation`</li>
    </ul>

<p align="center">
<img src="https://i.imgur.com/B47Bcnc.png" height="80%" width="80%" alt="UI to add Role"/>
</p>
<br />

<p>On the 'Select drivers to install step', hit `OK`. This will load the drivers from the Virtio-Win ISO. Select the driver that corresponds to the version of Windows you're installing. In this case we will select 2k22. Hit `Next`.</p>
<p align="center">
<img src="https://i.imgur.com/5BC7dZT.png" height="80%" width="80%" alt="UI to add Role"/>
</p>
<br />

<p>On the 'Where do you want your OS' screen, hit `next`. Then just wait for the installation process. It may reboot a few times, which is normal.</p>
<p align="center">
<img src="https://i.imgur.com/PS1YVxn.png" height="80%" width="80%" alt="UI to add Role"/>
</p>
<hr />

<p>Once the OS is installed onto the VM hard drive, we need to install the drivers from the Virtio-Win ISO. Login with credentials and go into the Virtio-Win ISO. Run "virtio-win-guest-tools" and install.</p>
<p align="center">
<img src="https://i.imgur.com/2PbRwUP.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />

<p>With the drivers installed the vm should run more smoothly and the desktop can now auto-resize.</p>
<p align="center">
<img src="https://i.imgur.com/uQesWIt.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />

<p>Final thing to do is renaming the server computer to something more sensible. Restart the vm afterwards.</p>
<p align="center">
<img src="https://i.imgur.com/fT2xHde.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<hr />

<p>Now time for some clean-up.</p>
    <ul>
        <li>Remove the Windows Server 2022 iso from CDROM 1</li>
	    <li>Remove CDROM 2</li>
    </ul>
<p align="center">
<img src="https://i.imgur.com/NKPakQD.png" height="60%" width="60%" alt="UI to add Role"/>
<img src="https://i.imgur.com/sgmXEHj.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<hr />
<p align="center"><b>Our server vm is now ready for the lab.</b></p>
</details>

<h2>Virtual Machine - Client</h2>

<details>
 <summary><b>- Configuration</b></summary>
 <p align="center">
<b>Creating the client VM is almost exactly the same as the server but with a few changes.</b>
</p>
<p>Create a new vm and do `local install`. Browse for the Windows 10 ISO. Go through the steps as before. On step 5 Network selection configure as below.</p>
<p align="center">
<img src="https://i.imgur.com/aOPUvt2.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>For new hardware, we only need a new CDROM for the Virtio-Win drivers.</p>
<p align="center">
<img src="https://i.imgur.com/VvY6rXU.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<hr />
<p align="center"><b>After reviewing the configuration we can begin the installation.</b></p>
</details>  

<details>
 <summary><b>- OS Installation</b></summary>
 <p align="center">
<b>This time we're installing Windows 10 Pro. It's only slightly different than installing Windows Server 2022.</b></p>
<p>After selecting our Windows version (Windows 10 Pro) & Custom Installation, we load our drivers. Select 'w10' for the drivers.</p>
<p align="center">
<img src="https://i.imgur.com/QOTi5op.png" height="60%" width="60%" alt="UI to add Role"/>
</p>
<br />
<p>Hit `next` to begin the installation.</p>
<p align="center">
<img src="https://i.imgur.com/Z7L8JOX.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<hr/>
<p>Since our network interface card is configured to our bridge it we won't have internet. Proceed by selecting `I don't have internet` -> `Continue with limited setup`.</p>
<p align="center">
<img src="https://i.imgur.com/IN8kVoQ.png" height="50%" width="50%" alt="UI to add Role"/>
<img src="https://i.imgur.com/BgDezlS.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>Unchecking will give some performance boost due to having less services running. If you don't care then just hit `Accept`.</p>
<p align="center">
<img src="https://i.imgur.com/CVGmG0E.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>Once logged on, install virtio-win-guest-tools.</p>
<p align="center">
<img src="https://i.imgur.com/KDWwLtF.png" height="50%" width="50%" alt="UI to add Role"/>
</p>
<br />
<p>Don't worry about renaming the pc right now. We will do that later in the lab. Cleanup is the pretty much same as with the server installation.</p>
<ul>
    <li>Remove the 'Windows 10 Multi-Edition' iso from CDROM 1</li>
    <li>Remove CDROM 2</li>
</ul>
<hr />
<p align="center"><b>And now we have our client pc for our lab!</b></p>
</details>  
