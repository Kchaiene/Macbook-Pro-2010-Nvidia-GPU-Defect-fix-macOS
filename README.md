## MacBook 2010 NVIDIA dGPU issue
#####This is other one method to solve MacBook 2010 NVIDIA dGPU issue (besides [These]( https://forums.macrumors.com/threads/gpu-kernel-panic-in-mid-2010-whats-the-best-fix.1890097/#post-23312990)), which based on approach for [MacBook 2011](https://gist.github.com/cdleon/d1eff7246a25193304284ecec40445b0).

#####In a nutshell, the difference is in the nvram GUID/UUID and NVIDIA dGPU 'kext' files :
 - use **'000fa4ce-b62f-4c99-9cc3-6815686e30f9'** instead of *'fa4ce28d-b62f-4c99-9cc3-6815686e30f9'*
 - move out only **GeForce.kext** files from */System/Library/Extensions*
 
  
 ## Full process flow
 
1. **Shutdown computer.**  
2. **Power up and boot into Single User Recovery by holding**  
   > if you are on high sierra 10.13.6+ you might need to use   Command + r instead
   
        Command + r + s
        
3. **Disable SIP (This takes a bit to complete so wait for it)**
 
        csrutil disable
 
4. **Disable Discrete GPU on boot by running**
 
        nvram 000fa4ce-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
    
5. **Reboot**
 
        reboot
 
6. **Boot into Single User-mode by holding**
 
        Command + s
  
7. **Mount root partition writeable**
 
        mount -uw /
       
    >**or if command not found**   
        
    >         /sbin/mount -uw /        
       
8. **Make a kext-backup directory**
     
        mkdir -p /System/Library/Ext-off
     
9. **Move offending kext out of the way**
     
        mv /System/Library/Extensions/GeForce.kext  /System/Library/Ext-off/
     
10. **Inform the system to update its kextcache:**
     
        touch /System/Library/Extensions/
     
11. **Reboot**
     
        reboot
      
12. **If the system works normally than Enable SIP again by restarting the system and entering Single User Recovery Mode**

    - **Power up and boot into Single User Recovery by holding** 
        > if you are on high sierra 10.13.6+ you might need to use Command + r instead

           Command + r + s

    - **Enable SIP**

           csrutil enable

    - **Restart the system**

           reboot  


  
  
## Updates Caveat and Warning  
When updating NVIDIA return the 'GeForce.kext' to its default location and moved it out again after update is complete.
  
After any system update the folder /System/Library/Extensions has to be checked for the offending 'GeForce.kext'.





