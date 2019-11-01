<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 10.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>
<a href="#gdcalert8">alert8</a>
<a href="#gdcalert9">alert9</a>
<a href="#gdcalert10">alert10</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


**Using the GemFire service connector **

**to Run and deploy an example PCC .NET app using Steeltoe**

Estimated completion time 20-30 mins.

**Prerequisites**



*   Working in Windows environment is optional, but highly recommended
*   Powershell
*   [Git](https://git-scm.com/downloads)
*   Visual Studio 2015+	
*   [CF CLI](https://pivotal.io/platform/pcf-tutorials/getting-started-with-pivotal-cloud-foundry/install-the-cf-cli)
*   Pivotal Network UAA Token 
    *   Log on to [network.pivotal.io](https://network.pivotal.io/)
    *   Click on your name tab and select "Edit Profile" to get your "API TOKEN" keys.

**Clone the Steeltoe GitHub Repository**

The Steeltoe repository has many example applications, we will use one such app to demonstrate the GemFire service connectors. To clone the git repo:


```
➜ git clone https://github.com/SteeltoeOSS/Samples.git
```


Now, we check out the appropriate branch.


```
➜ git checkout 2.x 
```


**Publish to Visual Studio**

Import the solution to Visual Studio to create needed directories, located at 


```
➜ \Samples\Connectors\src\AspDotNet4\Connectors.sln
```




<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the0.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the0.png "image_tooltip")


**Download GemFire Native **

Our Steeltoe application’s service connectors use the GemFire Native package to connect to our cache. We will use a script to obtain GemFire Native programmatically, but you may get it manually from the Pivotal Network [network.pivotal.io/products/pivotal-gemfire](http://network.pivotal.io/products/pivotal-gemfire). 

Open Powershell and run the script 


```
➜ .\GetGemFireDll.ps1
```


Then paste your Pivotal Network token like when prompted like the screenshot below.



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the1.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the1.png "image_tooltip")


The script will download a zip with the package  .\GetGemFire.dll. To unzip in powershell the command is:


```
➜ expand-archive -path .\pivotal-gemfire-native-10.0.2-build.9-Windows-64bit.zip
```


We will later paste  .\GetGemFire.dll into our published Steeltoe app

**Publish the app in Visual Studio**

Right click the GemFire4 directory in the Solution Explorer


    

<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the2.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the2.png "image_tooltip")


Select Publish from the context menu


    

<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the3.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the3.png "image_tooltip")


Make sure to publish to_ _the FolderProfile_ _and use this path as your Target Location_ _


```
bin/Debug/net461/win10-x64/publish
```


Then, to publish your app click the Publish button like the screenshot that follows.


    

<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the4.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the4.png "image_tooltip")



    _Note: Occasionally on some systems you may have to try the publish button more than once. For continued issues recheck the previous steps._

Now move the GemFire package into directory .\src\AspDotNet4\GemFire4\bin 


```
➜ Move-Item .\pivotal-gemfire-native-10.0.2-build.9-Windows-64bit\pivotal-gemfire-native\bin\Pivotal.GemFire.dll .\src\AspDotNet4\GemFire4\bin
```


**Connect your local GemFire to Pivotal Cloud Cache **

Login to your apps manager from the PCF CLI or if you are using PWS the process is the same.

First, create your Pivotal Cloud Cache instance. We are using the dev-plan and calling it pcc-steeltoe. Enter the following command and wait a few moments.


```
➜ cf create-service p-cloudcache dev-plan pcc-steeltoe
```



    _You can see the progress with the _cf service_s command_ cf s_ for short._

Next, make a service key we can call it pcc-steeltoe-servicekey. We will use this key to connect the app to our PCC instance _pcc-steeltoe_ .


```
➜ cf create-service-key pcc-steeltoe pcc-steeltoe-servicekey
```


Let’s look at our key. Enter the following,


```
➜ cf service-key pcc-steeltoe pcc-steeltoe-service-key
```


****Copy the first object called gfsh_login_string and save it in a text editor for later. You will use it to login to our PCC instance.**

**Push your application to the cloud **

Now, we can cf push our app from within the GemFire4 folder in the Samples repo  \Samples\Connectors\src\AspDotNet4\Gemfire4. 


    _Note: If you have issues make sure the service name in the manifest.yml matches the name of the PCC instance you created._



<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the5.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the5.png "image_tooltip")


Save the route from your deployed app so that you can visit your app when we finish configuring the cache.

**Download GemFire Binaries **

To get the GemFire Binaries go to [network.pivotal.io/products/pivotal-gemfire](http://network.pivotal.io/products/pivotal-gemfire). There you can download the **Pivotal GemFire Tar**. Don’t forget to sign in before downloading GemFire.


    

<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the6.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the6.png "image_tooltip")


When the download finishes unzip the tar. 

**Initialize the GemFire shell **

In the command prompt, paste the path of the GemFire folder you extracted to initialize your gfsh instance. For me this was at C:\Users\ivan\Desktop\fork\pivotal-gemfire-9.8.3\bin\gfsh you can also add this to your bash script. 



<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the7.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the7.png "image_tooltip")


**Log in to the GemFire shell**

Paste the “gfsh_login_string" you saved previously from the step above _Connect your local GemFire to Pivotal Cloud Cache_. The gfsh terminal will show _Cluster-0_ when you successfully log in.


```
Cluster-0 gfsh> create region --name=SteeltoeDemo --type=PARTITION
```




<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the8.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the8.png "image_tooltip")


**Your new PCC Steeltoe application!**

You can now go to the route created by Cloud Foundry and interact with your Pivotal Cloud Cache. There you will see the UI from your .NET app and you can interact with the cache of this basic application. From here you have seen how to customize your .NET Steeltoe app to have PCC capabilities. You can keep playing with this app to keep learning about PCC and Steeltoe



<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Using-the9.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Using-the9.png "image_tooltip")

