---
date: "2026-09-28T08:00:00-06:00"
title: "MYDFIR 30-Day Elastic Challenge: Install Elastic Agent for Ubuntu"
tags: ["DFIR", "Elasticsearch", "Elastic Agent", "Ubuntu", "Fleet", "Homelab"]
summary: "Install and enroll Elastic Agent on an Ubuntu VM to collect system logs and monitor the host in Elastic."
weight: 5
draft: true
---

## Introduction

In the previous section, we configured integrations to collect Sysmon and Windows Defender events from our Windows Server 2019 VM.

In this section, we will install the **Elastic Agent** on our Ubuntu VM and enroll it in Fleet so that we can monitor the host and collect system logs.

## Install Elastic Agent

### Step 1: Access the Ubuntu VM

Log in to the Ubuntu VM using the terminal or SSH.

[![Ubuntu terminal](Ubuntu_terminal.png)](Ubuntu_terminal.png)

### Step 2: Open Fleet in Kibana

Navigate to **Management → Fleet → Agents**.

Click **Add agent** to begin the enrollment process.

[![Add agent](Add_agent.png)](Add_agent.png)

### Step 3: Configure the Agent

Select the appropriate agent policy for the Ubuntu VM.

For this lab, we will use **[Ubuntu agent policy name]**.

[![Select agent policy](Select_agent_policy.png)](Select_agent_policy.png)

### Step 4: Download and Install Elastic Agent

Select Linux as the operating system and follow the installation instructions provided by Fleet.

Download and extract the Elastic Agent package using the commands displayed in Kibana.

[![Download Elastic Agent](Download_Elastic_Agent.png)](Download_Elastic_Agent.png)

Run the installation command provided by Fleet to enroll the agent.

[![Install Elastic Agent](Install_Elastic_Agent.png)](Install_Elastic_Agent.png)

### Step 5: Verify the Installation

Once the installation is complete, verify that the Elastic Agent service is running.

```bash
sudo systemctl status elastic-agent
```

[![Elastic Agent service status](Elastic_Agent_service_status.png)](Elastic_Agent_service_status.png)

## Verify the Agent in Fleet

Go back to **Management → Fleet → Agents** in Kibana.

Locate the Ubuntu agent and confirm that its status is **Healthy**.

[![Ubuntu agent healthy](Ubuntu_agent_healthy.png)](Ubuntu_agent_healthy.png)

## Verify Ubuntu Logs in Discover

Open **Analytics → Discover** in Kibana.

Select the appropriate data view and search for events from the Ubuntu VM.

If available, use the following KQL filter:

```kql
agent.name: "MYDFIR-UBUNTU"
```

[![Ubuntu logs in Discover](Ubuntu_logs_Discover.png)](Ubuntu_logs_Discover.png)

Verify that the events are being received and that the agent information matches your Ubuntu VM.

## Troubleshooting

If the agent does not appear in Fleet or no logs are visible in Discover, check the following:

- Confirm that the Elastic Agent service is running.
- Verify that the Ubuntu VM can reach the Fleet Server and Elasticsearch endpoints as required.
- Check that the correct agent policy was selected.
- Confirm that the agent is healthy in Fleet.
- Verify that the selected data view and time range include the expected events.

## Conclusion

In this section, we installed Elastic Agent on our Ubuntu VM and enrolled it in Fleet.

We verified the agent's health and checked that Ubuntu logs were being received in Kibana Discover.

With both Windows and Ubuntu hosts connected to our Elastic environment, we can continue expanding our lab and collecting telemetry from multiple systems.

## Next Steps

In the next section, we will continue building our Elastic lab and exploring the data collected from our endpoints.
