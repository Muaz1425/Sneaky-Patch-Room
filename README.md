
# Sneaky Patch

Writeup about TryHackme Sneaky Patch Challenge Room

[Diffuculty: Easy]
[Blue Team]

https://tryhackme.com/room/hfb1sneakypatch




## Scenario

![App Screenshot](https://github.com/Muaz1425/Sneaky-Patch-Room/blob/main/Images/SneakyPatchScenario.png)

From the Scenario, "Suspicious activity within the kernel" we know the flag would be inside the kernel. With that, we can start investigation by looking at kernel modules that loaded in memory.


## Walkthrough

Using the command "lsmod" we can list all kernel modules that currently loaded in memory, this is the best way to start the investigation by checking any anomaly in kernel.

![App Screenshot](https://github.com/Muaz1425/Sneaky-Patch-Room/blob/main/Images/SneakyPatchCommand1.png)

From the list kernel modules, we can identify which module is not the standard kernel module and can investigation further from there.

Here can see the non-standard module is spatch which we can start investigate the module further.

Using the command "modinfo spatch", we can display metadata of the kernel module which very useful to check for legitimate of the module.

Signer is shows that it legitimate signed by the trust entitity, blank signer shows that it from unknown origin and can be alarming for secure environment especially when secure boot is enabled which systems only allowed trusted signed kernel modules to run.

spatch module

![App Screenshot](https://github.com/Muaz1425/Sneaky-Patch-Room/blob/main/Images/SneakyPatchCommand2.1.png)

stp module

![App Screenshot](https://github.com/Muaz1425/Sneaky-Patch-Room/blob/main/Images/SneakyPatchCommand2.2.png)

From signer, we can see spatch signer is empty compare to stp which have a signer. Since spatch doesn't have signer, we can further investigate the spatch.

Using the command "Strings /lib/modules/6.8.0-1016-aws/kernel/drivers/misc/spatch.ko" we can extract embedded readable text from spatch file to see anythings that raise suspicious. 

![App Screenshot](https://github.com/Muaz1425/Sneaky-Patch-Room/blob/main/Images/SneakyPatchCommand3.png)

Here we able to see some bash that spatch will be execute and string that might be the flag. We can see there is encoding string and we can try decode it using cyberchef.

![App Screenshot](https://github.com/Muaz1425/Sneaky-Patch-Room/blob/main/Images/SneakyPatchDecode.png)

After decoding it using (From Hex), we can see the flag and completed the room.
