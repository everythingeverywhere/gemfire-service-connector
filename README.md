**Using the GemFire service connector**

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

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image1.png" width="624" height="318.67">

**Download GemFire Native**

Our Steeltoe application’s service connectors use the GemFire Native package to connect to our cache. We will use a script to obtain GemFire Native programmatically, but you may get it manually from the Pivotal Network [network.pivotal.io/products/pivotal-gemfire](http://network.pivotal.io/products/pivotal-gemfire). 

Open Powershell and run the script 


```
➜ .\GetGemFireDll.ps1
```


Then paste your Pivotal Network token like when prompted like the screenshot below.

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image6.png" width="124" height="104">

The script will download a zip with the package  .\GetGemFire.dll. To unzip in powershell the command is:


```
➜ expand-archive -path .\pivotal-gemfire-native-10.0.2-build.9-Windows-64bit.zip
```


We will later paste  .\GetGemFire.dll into our published Steeltoe app

**Publish the app in Visual Studio**

Right click the GemFire4 directory in the Solution Explorer

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image4.png" width="226.50" height="175.82">

Select Publish from the context menu

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image3.png" width="366" height="193.22">

Make sure to publish to_ _the FolderProfile_ _and use this path as your Target Location_ _


```
bin/Debug/net461/win10-x64/publish
```


Then, to publish your app click the Publish button like the screenshot that follows.

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image9.png" width="463.47" height="255.50">



    _Note: Occasionally on some systems you may have to try the publish button more than once. For continued issues recheck the previous steps._

Now move the GemFire package into directory .\src\AspDotNet4\GemFire4\bin 


```
➜ Move-Item .\pivotal-gemfire-native-10.0.2-build.9-Windows-64bit\pivotal-gemfire-native\bin\Pivotal.GemFire.dll .\src\AspDotNet4\GemFire4\bin
```


**Connect your local GemFire to Pivotal Cloud Cache**

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

**Push your application to the cloud**

Now, we can cf push our app from within the GemFire4 folder in the Samples repo  \Samples\Connectors\src\AspDotNet4\Gemfire4. 


    _Note: If you have issues make sure the service name in the manifest.yml matches the name of the PCC instance you created._


<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image8.png" width="694" height="60">


Save the route from your deployed app so that you can visit your app when we finish configuring the cache.

**Download GemFire Binaries**

To get the GemFire Binaries go to [network.pivotal.io/products/pivotal-gemfire](http://network.pivotal.io/products/pivotal-gemfire). There you can download the **Pivotal GemFire Tar**. Don’t forget to sign in before downloading GemFire.

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image2.png" width="377" height="318">

When the download finishes unzip the tar. 

**Initialize the GemFire shell**

In the command prompt, paste the path of the GemFire folder you extracted to initialize your gfsh instance. For me this was at C:\Users\ivan\Desktop\fork\pivotal-gemfire-9.8.3\bin\gfsh you can also add this to your bash script. 

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image10.png" width="386" height="132">

**Log in to the GemFire shell**

Paste the “gfsh_login_string" you saved previously from the step above _Connect your local GemFire to Pivotal Cloud Cache_. The gfsh terminal will show _Cluster-0_ when you successfully log in.

Create your SteelToeDemo Region with the type partition to begin to see the cache interacting with your app.
```
Cluster-0 gfsh> create region --name=SteeltoeDemo --type=PARTITION
```

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image5.png" width="624" height="177">

**Your new PCC Steeltoe application!**

You can now go to the route created by Cloud Foundry and interact with your Pivotal Cloud Cache. There you will see the UI from your .NET app and you can interact with the cache of this basic application. From here you have seen how to customize your .NET Steeltoe app to have PCC capabilities. You can keep playing with this app to keep learning about PCC and Steeltoe

<img src="https://github.com/everythingeverywhere/gemfire-service-connector/blob/master/assets/images/image7.png" width="624" height="325">
