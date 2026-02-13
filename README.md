
# Tweaking Huawei HG8145X6-10 Series ONT/ONU
The Huawei HG8145X6-10 is a mass produced ONT.
This repo was inspired by [@Anime4000](https://github.com/Anime4000/Hacking_Huawei_HG8240H5_ONT).

# Prerequisites
Prior access to Super Admin acccount required. 
With my ISP, a simple request through customer service gets you the account.

## Optional: an ONT you can play with.
I got another ONT with the exact same model for experimentation, so I don't risk my working ONT.

# Configuration File Retrieval & Manipulation
Inside the WebUI, go to System Tools, then Configuration File. It should let you download the config file. With this model, no need to use `aescrypt2_huawei` to decrypt the config file. Modify these and replace.

### ONT Services:
Change `0` to `1` for attribute: `SSHLanEnable="1"`. In both my ONTs, there's no `TELNETLanEnable="1" TELNETWanEnable="0"` attributes. These can safely be added, but TELNET DOES NOT work.
```xml
<AclServices HTTPLanEnable="1" HTTPWanEnable="0" FTPLanEnable="1" FTPWanEnable="0" TELNETLanEnable="1" TELNETWanEnable="0" SSHLanEnable="1" SSHWanEnable="0" HTTPPORT="80" FTPPORT="21" TELNETPORT="23" SSHPORT="22" HTTPWifiEnable="1">
<WanAccess NumberOfInstances="0"/>
<X_HW_RemoteAccess Enable="0" Port="" Protocol="" SupportedProtocols="HTTP,HTTPS,TELNET,FTP"/>
<X_HW_LocalAccess Enable="1" Port="80,443" Protocol="HTTP,HTTPS" SupportedProtocols="HTTP,HTTPS,TELNET,FTP"/>
</AclServices>
```

### ONT Accounts:
Add new attribute `<X_HW_CLISSHControl Enable="1" port="22" Mode="0" AluSSHAbility="0"/>` before `<X_HW_CLITelnetAccess/>`. For the root account password, copy your known account's hashed password and salt. No need to decrypt.

Say, we copy redactedUser's
```Password="$2WM(0+5yaO4;x)m+x3X7VJSX[@q_D@H#bxn+\+6f&amp;mKOBYA/%&amp;K~)RcYjrB7={%|};_gjZ@:=)%,&quot;t=tW`O^5Wj$%D-X46YQ!iH&gt;E$"``` and ```Salt="KsXQw3agnBN18BVgUbu80pvr"``` 
into root's
```Userpassword="$211e$LpZ`P3NI3NC.+x025rO)GrxpQ!&gt;*Px,|#2q=Q|Y8F.)AEG&apos;]0\&quot;KFm4=e1lt5%&lt;g&quot;K6&apos;:&gt;D&amp;91HPei:4Y`!)3*52Q9U}XS9G$"``` and ```Salt="KsXQw3agnBN18BVgUbu80pv"```. 

You are able to change the `UserName="redactedUser"` attribute as you like. 

`UserLevel="1"`, `WebIp="https://192.168.1.1"`, and `AdminWebIp=""` however, reverts to default when changed.

```xml
<UserInterface>
<X_HW_CLITelnetAccess Access="1" TelnetPort="23"/>
<X_HW_CLIUserInfo NumberOfInstances="1">
<X_HW_CLIUserInfoInstance InstanceID="1" Username="root" Userpassword="$211e$LpZ`P3NI3NC.+x025rO)GrxpQ!&gt;*Px,|#2q=Q|Y8F.)AEG&apos;]0\&quot;KFm4=e1lt5%&lt;g&quot;K6&apos;:&gt;D&amp;91HPei:4Y`!)3*52Q9U}XS9G$" UserGroup="" ModifyPWDFlag="0" EncryptMode="2" Salt="KsXQw3agnBN18BVgUbu80pvr" AccessInterface=""/>
</X_HW_CLIUserInfo>
<X_HW_WebUserInfo NumberOfInstances="2">
<X_HW_WebUserInfoInstance InstanceID="1" UserName="redactedUser" Password="$2WM(0+5yaO4;x)m+x3X7VJSX[@q_D@H#bxn+\+6f&amp;mKOBYA/%&amp;K~)RcYjrB7={%|};_gjZ@:=)%,&quot;t=tW`O^5Wj$%D-X46YQ!iH&gt;E$" UserLevel="1" Enable="1" ModifyPasswordFlag="1" WebIp="https://192.168.1.1" AdminWebIp="" Salt="KsXQw3agnBN18BVgUbu80pv" PassMode="3" Alias="cpe-1" AccessInterface="">
<X_HW_IteratePassword Password="$2WM(0+5yaO4;x)m+x3X7VJSX[@q_D@H#bxn+\+6f&amp;mKOBYA/%&amp;K~)RcYjrB7={%|};_gjZ@:=)%,&quot;t=tW`O^5Wj$%D-X46YQ!iH&gt;E$" Salt="GFedONUvaHYzTFTUISCtTMTP" IterateCount="10000" HashType="0"/>
<X_HW_History NumberOfInstances="1">
<X_HW_HistoryInstance InstanceID="1" X_HW_Password="$2WM(0+5yaO4;x)m+x3X7VJSX[@q_D@H#bxn+\+6f&amp;mKOBYA/%&amp;K~)RcYjrB7={%|};_gjZ@:=)%,&quot;t=tW`O^5Wj$%D-X46YQ!iH&gt;E$" X_HW_Time="20250508174653" X_HW_SaltVal="9sNBhASsM2llpXDp5/=zSm6m"/>
</X_HW_History>
</X_HW_WebUserInfoInstance>
<X_HW_WebUserInfoInstance InstanceID="2" UserName="redactedSuperAdmin" Password="$2WM(0+5yaO4;x)m+x3X7VJSX[@q_D@H#bxn+\+6f&amp;mKOBYA/%&amp;K~)RcYjrB7={%|};_gjZ@:=)%,&quot;t=tW`O^5Wj$%D-X46YQ!iH&gt;E$" UserLevel="0" Enable="1" ModifyPasswordFlag="1" WebIp="" AdminWebIp="https://192.168.1.1/admin.html" Salt="oGzFR2fb+PuGbF=A16ZXUq3F" PassMode="3" Alias="cpe-2" AccessInterface="">
<X_HW_IteratePassword Password="$2rDmm2o-l($1=&amp;@GLXg@+,.4&gt;)aYm$SJh/MIArcZApNPJ*Zu)/A;|32&lt;\=1k~hCKAR&amp;aqb&quot;@|1V5D@EpY&amp;H=7JuS_L@IYn*+zt]{G$" Salt="nY9Mg8K=rOnmsw=3ZPCxutGc" IterateCount="10000" HashType="0"/>
<X_HW_History NumberOfInstances="1">
<X_HW_HistoryInstance InstanceID="1" X_HW_Password="$2WM(0+5yaO4;x)m+x3X7VJSX[@q_D@H#bxn+\+6f&amp;mKOBYA/%&amp;K~)RcYjrB7={%|};_gjZ@:=)%,&quot;t=tW`O^5Wj$%D-X46YQ!iH&gt;E$" X_HW_Time="19810101000826" X_HW_SaltVal="KsXQw3agnBN18BVgUbu80pvr"/>
</X_HW_History>
</X_HW_WebUserInfoInstance>
</X_HW_WebUserInfo>
</UserInterface>
```

## Upload the Modified Config File
Save the modified config file and upload through WebUI > System Tools > Configuration File.  
The ONT will reject your config file if invalid changes are made. 


## SSH login
SSH to your ONT with root@[IP ADDRESS], and the password of the account you copied from.
Once inside, press `?` to list available commands.
typing `[command] ?` e.g., `acc get accesslimit ?` will show the usage guide, if any. 

Available commands for mine are:
```
acc get accesslimit
acc set accesslimit
add wifi filter
amp add policy-stats pon
amp add policy-stats port
amp add stats gemport
amp clear policy-stats pon
amp clear policy-stats port
amp clear stats gemport
amp del policy-stats pon
amp del policy-stats port
amp del stats gemport
ampcmd show emac stat
ampcmd show flow all
ampcmd show flow index
ampcmd show log
ampcmd trace all
ampcmd trace cli
ampcmd trace dpoe
ampcmd trace drv
ampcmd trace emac
ampcmd trace emap
ampcmd trace eth
ampcmd trace gmac
ampcmd trace gmap
ampcmd trace onu
ampcmd trace optic
ampcmd trace qos
appmcmd debug
appmcmd regplat
appmcmd show
arping
bbsp add policy-stats btv
bbsp clear policy-stats btv all
bbsp clear policy-stats wan
bbsp del policy-stats btv
bbsp dfx verify edge-voip-wan all
bbsp dfx verify edge-voip-wan start
bbspara
bbspcmd
Broadband debug
Broadband display
btv start period-stats
btv stop period-stats
chipdebug
chipdebug clearall
chipdebug soc drop
chipdebug soc rx
chipdebug soc tx
clear amp pq-stats
clear ftrace
clear lastword
clear pon statistics
clear poncnt dnstatistic
clear poncnt gemport upstatistic
clear poncnt upstatistic
clear port statistics
clear rogue flag
clear sfwd drop statistics
collect debug info
component delete all
ctrgcmd cloud
ctrgcmd debug
ctrgcmd frmwork
ctrgcmd logread
ctrgcmd plugin
ctrgcmd send
ctrgcmd show
debug dsp down msg
debug dsp msg
debug dsp up msg
debug ifm
debug qoscfg
debug rtp stack
debug sample mediastar
debugging dsp diagnose
debugging dsp para diagnose
debugging dsp record
debugging dsp t38diag
del wifi filter
delete debug info
dhcp client attach
dhcp client detach
dhcp client6 attach
dhcp client6 detach
dhcp server pool config
dhcp server pool disable
dhcp server pool enable
dhcp server pool lease config
dhcp server pool option add
dhcp server pool option del
dhcp server pool option flush
dhcp server pool restart
diagnose
display access mode
display access system info
display aclservicesrule
display amp policy-stats pon
display amp policy-stats port
display amp pq-stats
display amp stats gemport
display apmChipStatus
display backup list
display batteryStatus
display bbsp log
display bbsp stats btv
display bbsp stats wan
display bms
display bmsxml crc
display board fingerprints
display board-temperatures
display board2Item
display boardItem
display bomItem
display boost mode
display broadband info
display connection
display connection all
display connection stats
display cpu info
display current-configuration
display cwmp debug
display cwmp status
display ddns info
display debug info dhcp6c
display debug info dhcp6s
display debug info pppoev6
display debug info ra
display debuglog info
display debugwifilog info
display device-cert info
display deviceInfo
display dhcp client
display dhcp client all
display dhcp client6
display dhcp client6 all
display dhcp server pool
display dhcp server pool all
display dhcp server pool option
display dhcp server static
display dhcp server user
display dhcp server user all
display dhcp_em result
display diagnose info
display dns proxy info
display dnsserver static domain
display dpst all
display dpst detail
display dsp channel para
display dsp channel running status
display dsp channel status
display dsp chip stat
display dsp codec status
display dsp interrupt stat
display dynamic route
display eaiinfo
display edge_ont info
display emdi multicast result
display epon ont info
display ethoam ma info
display ethoam md info
display ethoam mep info
display ethoam mep perf
display femPar info
display femPar version
display ffwd table
display file
display filter rf
display firewall rule
display flashlock status
display flow
display ftp config status
display ftrace
display igmp
display igmp config
display inner version
display ip interface
display ip neigh
display ip route
display ip6tables filter
display iptables filter
display iptables mangle
display iptables nat
display iptables raw
display jb grid status
display jb para
display lan mac filter
display lanmac
display lanport workmode
display last fail VAS
display lastword
display log info
display lxcconfig
display mac ap
display mac ap brief
display macaddress
display macaddress timer
display machineItem
display memory detail
display memory info
display microwave ctrl info
display msg-queue
display nas-db-log
display nat port mapping
display NCE info
display nff log
display np statistics
display oaml2shell ethvlan
display omci tr069 requestFile
display omci tr069 responsetFile
display onu info
display optic
display optmode
display plugin equipinfo
display policy route all
display pon statistics
display pon traffic-suppress
display poncnt dnstatistic
display poncnt gemport upstatistic
display poncnt upstatistic
display port statistics
display portrate
display portstatistics
display ppp interface
display pppoe client
display pppoe client all
display pppoe_em result
display productmac
display progress load
display psi lastword
display radio stats
display rf config
display rfpi
display rtp stack channel stat
display rtp stack chip stat
display rtp stack para
display rtp stack version
display sfwd drop statistics
display sfwd port statistics
display sn
display specsn
display ssd all lognum
display ssd log
display ssh authentication-type
display ssh-hostkey fingerprint
display startup info
display swm bootstate
display swm state
display sysinfo
display syslog
display system info
display timeout
display timer
display tr069 info
display ufwinfo
display usb devList
display user device
display version
display voicelinetest
display voip call log detail
display voip dsp jbdata
display voip dsp para diagnose state
display voip dsp para diagnose statistics
display voip dsp tonedetect
display voip dtmfdiag state
display voip dtmfsimpara
display voip info
display voip rightflag
display voip ring info
display voip rtpdiag
display voip tone info
display wan layer all
display waninfo
display waninfo all
display waninfo all detail
display wanmac
display wifi ap
display wifi associate
display wifi calibrate mode
display wifi filter
display wifi information
display wifi multicast
display wifi neighbor
display wifi pa type
display wifi radio
display wifi ratio
display wifi smartant status
display wifichip
display wlan config
display wlan staevent
display wlan stainfo
display wlanmac
display zsp version
emdi multicast monitor start
emdi multicast monitor stop
fbt show bssidinfo
firewall log
flush dhcp server pool
flush dnsserver cache
fttr dfx clear
fttr dfx showinfo
fttr gmacdrv execcmd
fttr print olt omci
fttr reg showinfo
fttr reset ont
fttr restore manufactory ont
fttr set debugont
fttr show
get ap pon peakrate
get ap retrans
get battery alarm status
get ip conntrack
get mac agingtime
get ont oamfrequency
get opm switch
get optic debug info
get optic par info
get optic phy type
get optic txmode
get poncnt upgemport
get port config
get port config all
get port isolate
get port linetest
get rogue status
get testself
get testvoltage
get wifi atm
get wifi para
get wlan advance
get wlan associated
get wlan basic
get wlan enable
get wlan isolate
get wlan stats
get wlan wps
ifconfig
igmp add mirror filter
igmp clear statistics
igmp del mirror filter
igmp disable
igmp enable
igmp get debug switch
igmp get flow info
igmp get global cfg
igmp get iptv
igmp get mirror filter ip
igmp get multilmac
igmp get port multicast config
igmp get statistics
igmp set debug switch
ip -6 neigh
ip -6 route
ip -6 rule
ip interface config
ip neigh
ip route
ip route show
ip rule
jvmcmd map histo
jvmcmd stack trace
jvmcmd stat
lan mac filter add
lan mac filter delete
lan mac filter disable
lan mac filter enable
lan mac filter flush
load fem par
load pack
load ssh-pubkey
logout
macaddress
make ssh hostkey
mid get
mid off
mid set
napt cli
netstat -na
nslookup
oam show eventlog
oamcmd clear log
oamcmd debug
oamcmd error log
oamcmd pdt show log
oamcmd show flow
oamcmd show log
omcicmd alarm show
omcicmd clear log
omcicmd debug
omcicmd error log
omcicmd mib show
omcicmd pdt show log
omcicmd pm show
omcicmd show flow
omcicmd show log
omcicmd show qos
osgicmd debug
osgicmd get debug
osgicmd plugin permission
osgicmd set debug
osgicmd show bundleresource
osgicmd show bundlestate
ping
plugcmd show state
plugcmd start
plugcmd stop
plugcmd uninstall
plugincmd debug
ppp interface config
ppp l2log disable
ppp l2log enable
pppoe client attach
pppoe client detach
qoscfg get
quit
reset
reset edgeont
restore backup
restore manufactory
route get default
save data
save log
session cli
set aifi
set ap retrans
set apssh
set aptelnet
set bbsp optimization
set boost
set collect
set cwmp debug
set downpon eth
set dpst
set ethportipg
set ethportmirror
set ftrace
set iaccess speed
set lanport qbuf
set led
set llid
set microwave ctrl
set NCE
set newparentalctrl
set newparentalctrl stats
set nniethqueues
set opticdata
set pon distance
set pon traffic-suppress
set port isolate
set portmirror
set ringchk
set ssid
set timeout
set userpasswd
set voice announcement
set voice dtmfmethod
set voicedebug
set voicedsploop
set voicelinetest
set voiceportloop
set voicesignalingprint
set voip clip
set voip dsptemplate
set voip dtmfdebug
set voip dtmfdetfilter
set voip dtmfdiag start
set voip dtmfdiag stop
set voip dtmfsimpara
set voip dtmfsimu start
set voip dtmfsimu stop
set voip fax T38
set voip faxmodem switch
set voip highpassfilter
set voip portgain
set voip rtpdiag
set voip sipprofile
set wifi ap optimize
set wifi atm
set wifi expert
set wifi filter
set wifi para
set wifi radio
set wlan isolate
set wlan staboost
set wlan wmm
sfwd port statistics
smartapi glog clear
smartapi glog display
smartapi glog set
smartapi slog set
ssh authentication-type
ssh remote
stats clear
stats display
su
telnet remote
test tr069 inform end
test tr069 inform start
traceroute
trafficdump
ubus monitor
udm clear log
udm show log
undo debugging dsp diagnose
undo debugging dsp para diagnose
undo debugging dsp record
undo debugging dsp t38diag
undo firewall log
user
voice net diagnose start
voice remote diagnose server set
voice remote diagnose set
vspa clear rtp statistics
vspa debug
vspa display conference info
vspa display digitmap info
vspa display dsp running info
vspa display dsp state
vspa display dsp template info
vspa display mg if state
vspa display mg info
vspa display online user info
vspa display port status
vspa display profilebody info
vspa display rtp statistics
vspa display service log
vspa display signal scene info
vspa display signal scene list
vspa display user call state
vspa display user status
vspa reset
vspa shutdown mg
wap list
wap ps
wap top
wifi del fem calibration
wifi del fem par
wifi keepalive add
wifi keepalive del
wifi keepalive get
wifi keepalive set
wifi smartant set
wificmd
```


## Optional Changes

### ONT Remote Management [WORKING]
Disable Remote Management if you desire. `EnableCWMP="0"`
```xml
<ManagementServer EnableCWMP="0" URL="redacted" Username="redacted" Password="$2u5-SPs=_w7V^&gt;a*6l&gt;aPZft|==e/s*d&lt;j,WEZr4V$" PeriodicInformEnable="1" PeriodicInformInterval="43200" PeriodicInformTime="" ParameterKey="0" ConnectionRequestURL="" ConnectionRequestUsername="ccs" ConnectionRequestPassword="$2#.+hWU=%ORPHK~6|E4B1~_*:&lt;9s)L2JLUN.DqVcP$" UpgradesManaged="0" KickURL="" DownloadProgressURL="" DefaultActiveNotificationThrottle="0" UDPConnectionRequestAddress="" UDPConnectionRequestAddressNotificationLimit="0" STUNEnable="0" STUNServerAddress="" STUNServerPort="0" STUNUsername="" STUNPassword="" STUNMaximumKeepAlivePeriod="0" STUNMinimumKeepAlivePeriod="0" NATDetected="0" ManageableDeviceNumberOfEntries="0" ManageableDeviceNotificationLimit="0" X_HW_EnableCertificate="0" X_HW_CertPassword="$2}=\7,O;T~V7/J+!N~t@7_R|!IW]|ZB,&apos;JS*Pus`H$" X_HW_DSCP="0" X_HW_CheckPasswordComplex="0" X_HW_PeriodicInformTime=""/>
```

### ONT Power Management [UNKNOWN EFFECTS]
Disable Power Management. `EnablePowerSavingMode="0"`
```xml
<X_HW_APMPolicy EnablePowerSavingMode="0">
```

### ONT Custom Info [NO DIFFERENCE]
Change `isp` info to `common` device.
```xml
<X_HW_ProductInfo originalVersion="V500R022C00SPC319A2410310428" currentVersion="V500R022" customInfo="COMMON" customInfoDetail="COMMON"/>
```

Enjoy!
