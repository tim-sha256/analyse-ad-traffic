**Analyse all requests sent from your mobile device while using any app**

Read more about the project at timsh.org

Fill in the form if you found something interesting in the requests - let's analyse all apps from the [list](https://docs.google.com/spreadsheets/d/1fJbNT-kmfuWUlIpYr9sduvjZS1ggrmhydCzoDlqaMaA/) together!

---
**Follow this process**

1. **Install mitmproxy** 
   https://docs.mitmproxy.org/stable/overview-installation/
   on mac, simply use `brew install mitmproxy`
   
   be aware that mitmproxy is recognised as malware by some antiviruses - looks scary but it's not. 
   
2. **Turn on developer mode on iPhone / Android device**
   I did the whole experiment on iOS so I will not include any specific instructions for Android (though they should be even easier). 
   https://developer.apple.com/documentation/xcode/enabling-developer-mode-on-a-device

   You will need to have developer mode turned on for the next step. 
   
3. **Configure mitmproxy and iphone to work together**
	1. Launch mitmproxy in terminal. 
	   I prefer to use mitmweb because the interface is actually very helpful for initial discovery (and understanding the scale of the RTB requests).
	   
	   Use this command to launch the proxy that by default won't listen to the traffic on your computer (or any other device) without manually connecting to a proxy. 
	   `mitmweb --listen-host 0.0.0.0 --listen-port 8080`
	   
	2. Now `ipconfig getifaddr en0` to find your computer local IP address. 
	   By the way, your iphone and computer must be in the same wifi network for all of this to work. 
	   
	3. Next, open the settings on iphone and setup manual proxy with:
	   `server` = the ip address you just found
	   `port` = 8080
	   
	4. On iphone, open browser and go to mitm.it 
	   further instructions are described here, TLDR: you need to install the certificate and enable full trust to be able to decrypt TLS-encrypted traffic. 
	   
	   https://jasdev.me/intercepting-ios-traffic
	   https://support.apple.com/en-us/102390

4. We're all set! 
   Now you're able to intercept and decrypt all traffic going through iPhone. 
   If you only want to record traffic coming from a specific app, close all apps, "Clear flows" in MitmWeb and then open the desired app. 
   
5. Take any app from the [list](https://docs.google.com/spreadsheets/d/1fJbNT-kmfuWUlIpYr9sduvjZS1ggrmhydCzoDlqaMaA) (or just any app)
   
   In order to download it from App Store, you might have to turn off proxy on iphone, download the app and then turn it on again and clear the flows. 
   
6. Open the app and wait / click / play - you'll immediately see hundreds of requests flowing in mitmweb. 
   
7. When you feel like there's enough (you could even leave it open or play for an hour or so to collect more data), close the app and switch off the phone, then in mitmweb press File → Save all. 
   
   This will give you a `flows` file - rename it as "appname.flow"
   
8. Open the [mitm_test.ipynb](https://github.com/tim-sha256/analyse-ad-traffic/blob/main/mitm_test.ipynb) - either in local Jupyter Notebook or in Google Colab, both work fine. 
   Further instructions are included in the file itself. 
   
9. Repeat steps 5-7 for as much apps as you need, just don't forget to clear the flows before each recording. 
   When you're done, press `Ctrl+C` in terminal to stop mitmproxy and turn off proxy on iphone. 
   If that's your main device, you also MUST turn off the certificate trust setting that you enabled before.

---

** Check the instructions in [visualise_domains.ipynb](https://github.com/tim-sha256/analyse-ad-traffic/blob/main/visualise_domains.ipynb) to create a visualisation of domain and subdomain frequency in the data - just like this one: **
<img width="1091" alt="Screenshot 2025-04-16 at 19 20 25" src="https://github.com/user-attachments/assets/d6a259f2-1962-4532-899f-1a4f6b0062d1" />

