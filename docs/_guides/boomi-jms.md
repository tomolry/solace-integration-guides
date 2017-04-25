---
layout: guides
title: Dell Boomi JMS
summary: The Boomi AtomSphere integration platform as a service (iPaaS) supports all your application integration processes – between cloud platforms, software-as-a-service applications, and on-premises systems. Your entire team has online access to a powerful range of integration and data management capabilities, that can be realized in a fraction of the time of legacy middleware technologies. 
icon: dell-boomi.png
links:
    - label: Blog Post - Using Solace with Dell Boomi
      link: https://solace.com/blog/devops/solace-boomi
---

## What does this guide do?

This guide will take you through the process of modifiying a Boomi JMS sample process to use Solace.

## Prerequisites

This guide assumes that:

-	You have access to a Dell Boomi account.
-	You have installed the Boomi Atom (runtime) on the OS of your choice
-	You have access to Solace JMS libraries (version 6.2 or above)
-	You have access to a Solace Messaging Router
-	The necessary configuration on the Solace Message Router is done. Configuration includes the creation of elements such as the message-VPN and the JMS Connection Factory.
-	If SSL connectivity is desired, you will need to ensure that the Solace Message Router is correctly configured with SSL certificate(s), and that you have obtained a copy of the trust store, and keystore (if necessary) from your administrator.

## Setup 

### Step 1: Copy the Solace JMS libraries to your Boomi Atom installation

Copy the Solace JMS libraries to the [Boomi Atom Install]/userlib/jms directory and recycle Atom. For example, the below shows the Solace files copied to the proper locaiton installed a Linux image running in Amazaon EC2.
  
![]({{ site.baseurl }}/images/dell-boomi-jms/lib-install.png)

### Step 2: Create a Solace JMS Connection

Create a new JMS connection under "Connectors" using "Generic JNDI" for the JMS Server Type. The below shows a new connections using the default Solace JMS connnection factory, VPN, and port settings.

![]({{ site.baseurl }}/images/dell-boomi-jms/create-jms-connection.png)

### Step 3: Create a new “Operation” associated with the Solace JMS connection
  
![]({{ site.baseurl }}/images/dell-boomi-jms/create-send-operation.png)

### Step 4: Create a JMS Queue

Create a new JMS queue in either the Solace CLI or SolAdmin. The below shows a JMS queue (MyQueue) created under the default VPN.
  
![]({{ site.baseurl }}/images/dell-boomi-jms/create-queue.png)

### Step 5: Edit the Boomi JMS Send Message Sample to use Solace JMS Connection and Send Operation
  
![]({{ site.baseurl }}/images/dell-boomi-jms/edit-process.png)

### Step 6: Use the Solace JMS consumer example to receive the message (SolJMSConsume.java is packaged with the Solace JMS libraries). Below are the arguments used to run the sample - edit the IP to the Solace Router accordingly. Execute the sample to begin receiving messages.

-username default -url smf://52.201.252.190 -cf /jms/cf/default -queue Q/MyQueue

### Step 7: Test the process using the "Test" button in the process viewer. You should see 3 messages received in the SolJMSConsumer application.

![]({{ site.baseurl }}/images/dell-boomi-jms/test-process.png)

