# laptop-router
Converting a laptop into a router

This is my current router solution for my home network. I wasn’t satisfied with the performance of my cheap Cudy router with OpenWRT and I couldn’t find a commercially available product with the features I wanted for a reasonable price, so I decided to build my own.

![PXL_20220913_155846732-1024x768](https://github.com/svolovar/laptop-router/assets/72886129/6e623283-204a-4273-a2f4-56dd73d06101)

I found a used Lenovo C940 laptop for a heavily discounted price due to the cracked screen and modified it to accept a PCIE NIC. I separated the screen which I later converted into a standalone monitor, and used the base of the laptop as a headless computer for the router.

This laptop was a good candidate for this build for several reasons:

    It was afforable
    The Intel Ice Lake CPU is powerful and efficient
    The NVME slot can accept an adapter which allowed me to connect a PCIE NIC
    The two Thunderbolt 3 ports provide easy IO expansion
    USB-C charging makes it easy to source a power supply
    The Intel chipset and the rest of the hardware has good linux support

Using a laptop as a router is unconventional and required heavy modifications, but the result is a router with ample resources and an integrated back-up battery. Also, the built-in keyboard and display output makes it easy to troubleshoot if I am ever unable to access the router over the network.

To get an idea of what was involved in modifying the laptop, here are some pictures of the major steps I took:

![PXL_20220906_183344116-1024x768](https://github.com/svolovar/laptop-router/assets/72886129/3a765bc0-8df1-4dd4-8021-526b6ac285e8)
Slot cut out of bottom plate to accept PCIE to NVME adapter, screws replaced with standoffs for mounting of acrylic base plate, angled bracket bolted on to bottom plate for installation of PCIE NIC.


![PXL_20220906_183149285-edited-1-2048x1537](https://github.com/svolovar/laptop-router/assets/72886129/ed074ee6-620b-4ecb-ae43-425c006badcd)
PCIE to NVME riser installed into NVME slot and bolted onto the angled adapter with standoffs


![PXL_20220906_181855266-1-edited-2048x1279](https://github.com/svolovar/laptop-router/assets/72886129/91fa989f-3924-4143-8241-cdfdb00afffe)
Four port Intel NIC installed into PCIE riser


For the operating system, OpenWRT is installed on a USB 3.0 flash drive connected to an angled USB adapter cable so it can be stored neatly between the laptop and the acrylic base. This reduces the possibility of someone accidentally removing the drive and rendering the router inoperable. Though I was concerned about using a flash drive as a main drive for the operating system at first, I learned that OpenWRT loads itself into the RAM on boot. Therefore, the amount of read/write cycles is minimal so even in the worst case scenario of the flash drive failing at it’s minimum rated amount of read/write cycles, I should still have years of use before drive failiure is a concern.

For multi-wan and failover functionality, I installed and configured an OpenWRT package called MWAN3. This allowed me to assuign two ethernet ports on the NIC as WAN ports. The WAN connections are provided from two ISPs. One is a cable internet connection coming from a cable modem, and the other is a 4G/5G cellular connection provided through a Quectel RM520N-GL modem installed in a board which has an ethernet output. The redundancy provided by these two connections is robust because they rely on two entirely separate infrastructures. The router defaults to routing traffic through the cable modem by default and if that connection goes down, packets are automatically diverted to the hotspot. This failover functionality is fast, seamless and automatic. While connected to the network, users don’t notice any interuption in service when the main connection goes down.

The result is a router with more than enough resources for my home network. It can easily handle custom firewall rules and multiple WAN connections, and VPN functions. The CPU isn’t even remotely a bottleneck when it comes to VPN connections, either through OpenVPN or Wireguard. The excess resources give me the freedom to install OpenWRT packages for QOS, IDS and IPS, and custom DNS configurations without ever having to worry about high CPU utilization, at least for the foreseeable future.
