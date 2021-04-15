# Fleet Agent Client Install

This module will walk you through on the install of the fleet agent in your windows system.

First thing first, let's download the config file.

Main page for Elastic Agent.
- Main Download page:

https://www.elastic.co/downloads/elastic-agent

Or the file download it's self!

Note: Download the Agent configs for x64 bit zip file. 

- Windows x64 shortcut download:

https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.12.0-windows-x86_64.zip


![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/agent-config.png)


![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/powershell2.png)


- Now extract this archive into its same directory in **Downloads:**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/save.png)

- Now copy the **elastic-agent-7.12.0-windows-x86_64** to **C:\ Program files**


![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/save2.png)

- Click **Continue** to save:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/save3.png)

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/save4.png)
- Now run **powershell** as administrator:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/powershell.png)


- Now copy these commands in powershell to go to this directory:

~~~
cd /
cd '.\Program Files\'
Cd elastic-agent-7.12.0-windows-x86_64

or It may have a double directory like below:

cd elastic-agent-7.12.0-windows-x86_64\elastic-agent-7.12.0-windows-x86_64
~~~

- Note: To copy and paste within **powershell** right click in top left hand corner and go to **edit** then **paste**:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/powershell3.png)

### Now let's get that **enrollment token**:

Go back to the main menu in the Kibana Dashboard:

- Now click on **Add Elastic Agent:**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/agent.png)

- Now click on **Add Agent:**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/agent2.png)

1. Now click on **Enroll in Fleet:**
2. Click on **Agent Policy** and select your policy:


![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/agent3.png)


3. Copy the: Enroll and start the **Elastic Agent for windows** and click continue:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/agent4.png)

Then go back to your **powershell window** and paste the token and hit enter:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/powershell4.png)

Once you hit enter, you should see a message below like this one:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/works.png)

If so it works!!

# ElasticSIEM Fleet Agent Enrolled!!
### Congradulations!!!!!


### Now go back you your dashboard.

- Now click on **View agents**:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/works2.png)

- Then you should see your Fleet Agent Enrolled:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/works3.png)


If everything is **Healthy** and green it is installed and working.

- Now go back to the main dashboard by clicking on the **elastic Icon** then **Security**:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/works4.png)


![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/works5.png)

- Now go to **Timelines:**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/works6.png)

- Now click on **Create new timeline:**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/timeline.png)

- Now type in the **Search box:** **event.module : "endpoint"**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/timeline2.png)

Nothing should show up unless your system has been active on the internet.

So lets make some detection alerts.

- Now go back to powershell and type these commands:

~~~
cmd.exe
powershell.exe
whoami
~~~

These commands should trigger some alerts.

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/alerts.png)


- Now lets go back to our Kibana Dashboard and click **Refresh**:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/alerts2.png)

This may take a moment but some data should show up and make sure your time range is set to **Last 24 hours**

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/24.png)

As you see above, we have a **Detection Alert** for **whoami** and powershell command.

- Note: Mines showed up late it should be **cmd.exe** **powershell.exe** and **whoami**

- Now click on the first alert:

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/detect.png)

## Got It!!!

![](https://github.com/watsoninfosec/private/blob/main/Deployment-Guide/Fleet-Agent/screenshots/gotit.png)

Note: You should see a graph like this above. That will help you investigate threats on your systems or network.

# This where your **"Threat Hunting Begins!"**

#### Your ElsticSIEM Has Been Configured and Complete!!