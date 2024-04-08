PART 1
~~~
# INSTALL AUTO TIME SYNC
opkg update && wget --no-check-certificate https://raw.githubusercontent.com/wifikunetworks/hgp/main/autotimesync.sh -O /usr/bin/autotimesync.sh && chmod +x /usr/bin/autotimesync.sh

# INSTALL SPEEDTEST
opkg remove --force-remove luci-app-speedtest ; rm /usr/bin/speedtest ; wget --no-check-certificate -P /root https://github.com/animegasan/luci-app-speedtest/releases/download/v1.3/luci-app-speedtest_1.3_all.ipk && opkg install --force-reinstall /root/luci-*-speedtest*.ipk && rm /root/*.ipk

# INSTALL TAILSCALE
wget --no-check-certificate -P /root https://github.com/wifikunetworks/hg680p/raw/main/luci-app-tailscale_1.0.5_all.ipk && opkg install --force-overwrite /root/luci-*-tailscale*.ipk && rm /root/*.ipk

# INSTALL ANTI BENGONG
wget --no-check-certificate -N -P /www/antibengong/ https://raw.githubusercontent.com/wifikunetworks/antibengong/main/lite.sh && chmod +x /www/antibengong/lite.sh

# INSTALL INTERNET DETECTOR
opkg remove --force-removal-of-dependent-packages internet-detector && rm /etc/config/internet-detector ; wget --no-check-certificate -O /tmp/internet-detector_1.3-0_all.ipk https://github.com/wifikunetworks/luci-app-internet-detector/raw/master/internet-detector_1.3-0_all.ipk && opkg install /tmp/internet-detector_1.3-0_all.ipk && rm /tmp/internet-detector_1.3-0_all.ipk ; /etc/init.d/internet-detector start && /etc/init.d/internet-detector enable && wget --no-check-certificate -O /tmp/luci-app-internet-detector_1.3-0_all.ipk https://github.com/wifikunetworks/luci-app-internet-detector/raw/master/luci-app-internet-detector_1.3-0_all.ipk && opkg install /tmp/luci-app-internet-detector_1.3-0_all.ipk && rm /tmp/luci-app-internet-detector_1.3-0_all.ipk ; /etc/init.d/rpcd restart && wget --no-check-certificate -O /tmp/internet-detector-mod-modem-restart_1.3-0_all.ipk https://github.com/wifikunetworks/luci-app-internet-detector/raw/master/internet-detector-mod-modem-restart_1.3-0_all.ipk && opkg install /tmp/internet-detector-mod-modem-restart_1.3-0_all.ipk && rm /tmp/internet-detector-mod-modem-restart_1.3-0_all.ipk ; /etc/init.d/internet-detector restart

# INSTALL CONNECTION MONITOR
opkg remove --force-remove luci-app-lite-watchdog && rm /etc/modem/log.txt ; wget --no-check-certificate -P /root https://raw.githubusercontent.com/wifikunetworks/hgp/main/luci-app-lite-watchdog_1.0.13-20231207_all.ipk && opkg install --force-reinstall /root/luci-*-watchdog*.ipk && rm /root/*.ipk

# INSTALL SMS TOOL
opkg remove --force-remove luci-app-sms-tool-js && rm /etc/config/sms_tool_js ; wget --no-check-certificate -P /root https://raw.githubusercontent.com/wifikunetworks/hgp/main/luci-app-sms-tool-js_2.0.20-20240201_all.ipk && opkg install --force-reinstall /root/luci-*-sms*.ipk && rm /root/*.ipk

# INSTALL OPENCLASH
wget --no-check-certificate -P /root https://github.com/wifikunetworks/hg680p/raw/main/luci-app-openclash_0.46.003-beta_all.ipk && opkg install --force-overwrite /root/luci-*-openclash*.ipk && rm /root/*.ipk
~~~
PART 2
~~~
wget --no-check-certificate -O /etc/config/system https://raw.githubusercontent.com/wifikunetworks/hg680p/main/system
wget --no-check-certificate -O /etc/config/wireless https://raw.githubusercontent.com/wifikunetworks/hg680p/main/wireless
wget --no-check-certificate -O /etc/config/atcmds.user https://raw.githubusercontent.com/wifikunetworks/hg680p/main/atcmds.user
wget --no-check-certificate -O /www/luci-static/material/brand.png https://raw.githubusercontent.com/wifikunetworks/hg680p/main/brand.png
wget --no-check-certificate -O /www/luci-static/resources/view/status/include/10_system.js https://raw.githubusercontent.com/wifikunetworks/hg680p/main/10_system.js
wget --no-check-certificate -O /etc/profile.d/30-sysinfo.sh https://raw.githubusercontent.com/wifikunetworks/hg680p/main/30-sysinfo.sh
wget --no-check-certificate -O /etc/crontabs/root https://raw.githubusercontent.com/wifikunetworks/hg680p/main/root
wget --no-check-certificate -O /etc/rc.local https://raw.githubusercontent.com/wifikunetworks/hg680p/main/rc.local
wget --no-check-certificate -O /usr/share/ucode/luci/template/sysauth.ut https://raw.githubusercontent.com/wifikunetworks/hg680p/main/sysauth.ut
wget --no-check-certificate -O /etc/banner https://raw.githubusercontent.com/wifikunetworks/hg680p/main/banner
wget --no-check-certificate -O /usr/share/ucode/luci/template/themes/material/header.ut https://raw.githubusercontent.com/wifikunetworks/hg680p/main/header.ut
wget --no-check-certificate -O /usr/share/ucode/luci/template/themes/material/footer.ut https://raw.githubusercontent.com/wifikunetworks/hg680p/main/footer.ut
wget --no-check-certificate -P /tmp https://raw.githubusercontent.com/wifikunetworks/hg680p/main/mm.ipk && opkg install --force-reinstall /tmp/mm.ipk

echo "chain mangle_postrouting_ttl65 {
  type filter hook postrouting priority 300; policy accept;
  counter ip ttl set 65
}

chain mangle_prerouting_ttl65 {
  type filter hook prerouting priority 300; policy accept;
  counter ip ttl set 65
}" > /etc/nftables.d/10-custom-filter-chains.nft
service firewall restart
~~~
