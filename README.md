# amplitude-crossdomain-tracking


# Instuctions
1. Use the same Amplitude Key across all the domains
2. Enable `deviceIdFromUrlParam` at amplitude.getInstance().init
```
amplitude.getInstance().init("AMPLITUDE_KEY", null, {
    deviceIdFromUrlParam: true
});
```
3. Add CrossDomain Tracking script 
```
(function(){
  
  	setTimeout(function(){ 

      var domainsList = ['example.com','blog.com','landingpages.com'];
      var destinationList = ['ecommerce.com'];

      if(window.location.host.includes(domainsList)){
          console.log('CrossDomain Tracking ON');
          var pageLinks = document.getElementsByTagName("a");
          var deviceId = amplitude.getInstance()._getInitialDeviceId();
          if(deviceId){
              var changed = 0;
              for (var i = 0; i < pageLinks.length; i++) {
                  if(pageLinks[i].href.includes(destinationList)){
                      changed++;
                      if(pageLinks[i].href.includes('?')){
                          pageLinks[i].href = pageLinks[i].href + "&"+"amp_device_id="+deviceId;
                      }else{
                          pageLinks[i].href = pageLinks[i].href + "?amp_device_id="+deviceId;
                      }

                  }
              }
             console.log('CrossDomain Tracking ON: Links changed '+changed);
          }else{
              console.log('CrossDomain Tracking OFF: deviceId undefined');
          }

      }else{
          console.log('CrossDomain Tracking OFF: Domain is not listed in domainsList');
      }
      
    }, 1500);
    
})()
```


