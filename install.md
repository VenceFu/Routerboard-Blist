

1. 安裝下載語法到RB :
/system script
add comment=Download-BlackList dont-require-permissions=yes name=\
    Blacklist_SquidBlacklist_Download_drop.malicious.rsc owner=vence policy=\
    read,write,test source="/tool fetch address=list.downus.me host=list.downu\
    s.me mode=https src-path=/blacklists.co.rsc dst-path=/blacklists.co.rsc"
add comment=Run-Impert-BlackList dont-require-permissions=no name=\
    Blacklist_SquidBlacklist_Import_drop.malicious.rsc owner=vence policy=\
    read,write source="import /blacklists.co.rsc\r\
    \n"

2. 安裝RAW阻擋到RB:
/ip firewall raw
add action=drop chain=prerouting src-address-list=vBlock

3. 安裝scheduler到RB
/system scheduler
add interval=1h name=BlackList_AutoDownload_Impert on-event="/system script r\
    un Blacklist_SquidBlacklist_Download_drop.malicious.rsc\r\
    \n/system script run   Blacklist_SquidBlacklist_Import_drop.malicious.rsc\
    " policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive \
    start-date=jan/01/2020 start-time=00:01:00


恭喜你，安裝完成
你可以手動嘗試執行 System Script裡面的 Download-BlackList 和 Run-Impert-BlackList
如果你有看到大量的IP被放到/ip firewall address lists ，那就是正常了
接著很快你就會看到 /ip firewall raw 的 vBlock開始有數字在跳了，代表阻擋成功
