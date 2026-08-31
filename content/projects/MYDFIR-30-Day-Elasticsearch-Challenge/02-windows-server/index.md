---
date: "2026-08-05T19:59:32-06:00"
title: "MYDFIR 30-Day Elastic Challenge: Windows Server"
tags: ["DFIR", "Windows Server", "VirtualBox", "Homelab"]
summary: "Deploying a Fleet Server and enrolling a Windows Server 2019 VM as an Elastic Agent for the MYDFIR 30-Day Elastic Challenge."
weight: 2
---

## Create a Fleet Server

We previously deployed an Ubuntu VM from [osboxes.org](https://www.osboxes.org/ubuntu/) to use as our Fleet Server.

To configure it as a Fleet Server, we go back to the Elastic web interface. From our analyst machine, we navigate to our Elastic server using a web browser.

From the hamburger menu, select **Management > Fleet** and click the blue **Add Fleet Server** button.

[![Add a Fleet Server](Add_a_Fleet_server.png)](Add_a_Fleet_server.png)

A Fleet Server is required before we can enroll Elastic Agents using Fleet.

From the side panel, select **Quick Start**. We enter `MYDFIR-FLEET-SERVER` as the Fleet Server name and specify the Fleet Server URL:

`https://172.31.0.10`

We then click **Generate Fleet Server Policy**.

[![Fleet Server Quick Start](Fleet_Server_Quick_Start.png)](Fleet_Server_Quick_Start.png)

After a couple of minutes, we receive confirmation that the **Fleet Server policy has been created**.

[![Fleet Server Policy Created](Fleet_Server_Policy_Created.png)](Fleet_Server_Policy_Created.png)

We can now proceed to Step 2, **Install Fleet Server to a centralized host**.

We will run the installation commands from Step 2 on our Ubuntu `MYDFIR-FLEET-SERVER` VM.

[![Fleet Server Installation Script](Fleet_Server_Installation_Script.png)](Fleet_Server_Installation_Script.png)

### Troubleshooting the Fleet Server installation

The installation initially failed. Looking closely at the installation output, I noticed that the script was downloading an agent package for the wrong architecture, which caused the installation to fail on my Fleet Server.

Initially, I thought the problem might be related to the user account, so I tried running the installation as `root`.

[![Fleet Server Failed Installation Script](Fleet_Server_Failed_Installation.png)](Fleet_Server_Failed_Installation.png)

However, the installation still failed.

[![Fleet Server Failed Installation Script](Fleet_Server_Failed_Installation_ARM.png)](Fleet_Server_Failed_Installation_ARM.png)

I then corrected the download to use the **x86_64** version of the Elastic Agent. I also removed the files from the previous installation attempt before trying again.

As Steven demonstrated in the Day 7 video, some troubleshooting was required because the generated installation command was not working correctly in my environment. The download portion worked, but the remaining commands did not.

Instead of running the entire command at once, I downloaded the correct x86_64 package and executed each step individually.

[![Downloading the x86_64 agent file](Downloading_the_x86_64_agent_file.png)](Downloading_the_x86_64_agent_file.png)

Once the agent package was downloaded, I extracted its contents, changed into the extracted directory, and proceeded with the installation.

[![Installing the Elastic Agent](Installing_the_elastic_agent.png)](Installing_the_elastic_agent.png)

The installation completed successfully.

[![Elastic Agent has been successfully installed](Elastic_agent_has_been_successfully_installed.png)](Elastic_agent_has_been_successfully_installed.png)

We can now return to the Elastic web interface and verify the Fleet Server enrollment. The interface shows the message **"Fleet Server connected."**

We can then click **Continue enrolling Elastic Agent** to proceed with adding our Windows Server.

[![Fleet Server connected](Fleet_Server_connected.png)](Fleet_Server_connected.png)

## Installing Elastic Agent on Windows Server

We previously deployed a Windows Server 2019 VM in VirtualBox with the name `MYDFIR-WIN`.

Now we can add an Elastic Agent to the Windows Server. From Fleet, click **Add** and select **Agent**.

[![Fleet Server - Add Agent](Fleet_Server_Add_Agent.png)](Fleet_Server_Add_Agent.png)

We create an agent policy named `MYDFIR-WIN-POLICY` and click **Create Policy**.

[![Creating Agent Policy](Creating_Agent_Policy.png)](Creating_Agent_Policy.png)

Interestingly, I received an error indicating that the policy already existed. I don't remember creating this policy previously.

[![Error Creating Agent Policy](Error_Creating_Agent_Policy.png)](Error_Creating_Agent_Policy.png)

I navigated to **Agent Policies** to see what policies were already configured, and I found the `MYDFIR-WIN-POLICY` policy already there. I clicked on it to inspect the configuration.

[![Fleet Server - Add Agent](Navigate_to_Agent_Policies.png)](Navigate_to_Agent_Policies.png)

Inside the policy, I clicked **Add Agent** to add the Windows Server agent.

[![MYDFIR-POLICY Add Agent](MYDFIR-POLICY_Add_Agent.png)](MYDFIR-POLICY_Add_Agent.png)

I scrolled down to the agent installation section, selected **Windows**, and copied the installation command.

[![Windows Server Agent Installation Code](Windows_Server_Agent_Installation_Code.png)](Windows_Server_Agent_Installation_Code.png)

### Troubleshooting the Windows Agent installation

Before running the installation command, I ran into several issues with the Elastic Agent installation.

I noticed that the Fleet Server URL and port in the generated command were not correct for my lab environment, similar to the issue Steven demonstrated in his video.

I changed the Fleet Server IP address to point to my Fleet Server:

`172.31.0.20`

I also changed the port from:

`443`

to:

`8220`

[![Fleet Server Host Configuration](Fleet_Server_Host_Configuration.png)](Fleet_Server_Host_Configuration.png)

With the Fleet Server configuration corrected, I copied the installation command to the Windows Server and ran it from an **elevated PowerShell** window.

For this lab, I also used the `--insecure` option and confirmed the installation by entering `Y` when prompted.

[![Windows PowerShell Agent Installation](Windows_PowerShell_Agent_Installation.png)](Windows_PowerShell_Agent_Installation.png)

The Elastic Agent was successfully installed on the Windows Server, allowing it to communicate with our Fleet Server.

We can verify the enrollment from the Fleet interface, where the Windows Server appears as an enrolled agent.

[![Windows Server enrolled to Fleet Server](Windows_Server_Enrolled_to_Fleet_Server.png)](Windows_Server_Enrolled_to_Fleet_Server.png)

We can also verify that logs are being collected by searching Elasticsearch for the Windows Server's `agent.name`.

[![Windows Server agent.name search](Windows_Server_agent-search.png)](Windows_Server_agent-search.png)

## Next Steps

With the Fleet Server and Windows Server agent configured, we are ready to continue with **Day 8** of the MYDFIR 30-Day Challenge.

The next step will introduce **Sysmon**, which will provide additional Windows telemetry and help us collect more detailed information for our DFIR and detection activities.
