# Useful links 

[OpenWrt description page](https://openwrt.org/toh/tp-link/archer_c7)

# General description

Ziggo C7 is a re-branded TP-Link Archer C7 v2.0. The devices is capable to run OpenWRT, as described on a link above. 
However, unlocking branded devices requires some home-crafted binaries. 

# Firmware upgrade 

To install OpenWRT one has first to reflash device with a stock firmware. Branded firmware expect different checksum, therefor stock firmware has to be patched. 
(tplink-ziggo-stock-patched.bin) - already patched image based on a lates available stock fw `ArcherC7v2_en_eu_3_15_3_up_boot(180305).bin`
This image could be flashed via WebUI. 

After flashing stock fw the deivce might be reflashed with OpenWRT downloaded from device page. However, the lates stock firmware check image version, and somehow blocks third-party images. If that's the case, one can downgrade `ArcherC7v2_en_eu_3_15_3_up_boot(180305).bin` to older image [ArcherC7v2_en_3_14_2_up_boot(150304).bin](ArcherC7v2_en_3_14_2_up_boot(150304).bin) - this image does not check version and checksums. 
The downgrade available with TFTP as described at OpenWRT website. 

After downgrading, the official OpenWRT image might be flashed via WebUI. 

