## Functional Requirements

1. 
In case application re-registers during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N`  
and sends valid `hashID` param via RegisterAppInterface_request to SDL  

SDL must:  
- respond RegisterAppInterface (SUCCESS, success:true) to mobile app (in case of no other failures)  
- keep all data created/stored by this app BEFORE re-registration (transport switching)

2.  
In case application re-registers during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N`  
and sends invalid `hashID` param via RegisterAppInterface_request to SDL  
OR omits `hashID` at all  

SDL must:  
- respond RegisterAppInterface (RESUME_FAILED, success:true) to mobile app (in case of no other failures)  
- clear all data related to this app internally except "AppIconsFolder"  
- send requests to remove data in HMI

3.  
In case application re-register AFTER `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` expiration  
and sends valid `hashID` param via RegisterAppInterface_request to SDL   

SDL must:  
- notify HMI about app being unregistered
- start data resumption process according to existing rules when app re-registers

4. 
In case application re-registers AFTER `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` expiration 
and sends invalid `hashID` param via RegisterAppInterface_request to SDL  
or omits `hashID` at all  

SDL must:  
behave according to existing Data Resumption rules 

## Non-Functional Requirements
New parameters "AppTransportChangeTimer" , "AppTransportChangeTimerAddition"   must be added to smartdevicelink.ini file -> [MAIN] section

```
[MAIN] 
"AppTransportChangeTimer" = 500 ms 
Defines the timeout period SDL Core will wait for an application to re-register via alternate transport during 'transport switch'. If the application fails to re-register within this time, SDL Core shall inform HMI that the application has unregistered.

```

```
[MAIN] 
"AppTransportChangeTimerAddition" = 0ms
Defines the timeout  for waiting of every mobile app to reconnect between Bluetooth and USB transports change in case the number of connected apps is more than one
```

## Diagram
Data resumption
![Data resumption](https://github.com/smartdevicelink/sdl_requirements/blob/feature/iap_over_bt/detailed_docs/accessories/iAP_over_BT_data_resumption.png)