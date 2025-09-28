Firewall: A firewall is a security system that acts as a barrier between two networks (or a single computer and the internet). Its purpose is to filter network traffic and decide what data is allowed to pass through based on a set of rules. It is the first line of defense against unwanted or malicious network connections.

Inbound Traffic: This refers to data that is coming into your computer from an external network or the internet. An inbound firewall rule controls what connections are allowed to reach services on your machine.

Initial Test and Failure:

My first step was to open PowerShell and check if I could connect to my own machine on port 23. I used the command:

__Test-NetConnection -ComputerName localhost -Port 23__

The result showed __TcpTestSucceeded : False__. I also tried the same with the telnet command, which showed "Connect failed."

*Why it Failed: I initially thought this was due to the firewall, but I learned that the problem was much simpler: no service was running on port 23 to accept the connection. A firewall rule is irrelevant if there is no program listening on that port. This was my first key learning: a firewall can't open a port if there's nothing behind the door.*

**Creating the Firewall Rule**

I opened Windows Defender Firewall with Advanced Security.

I went to Inbound Rules and selected New Rule.

<img width="1914" height="889" alt="Screenshot 2025-09-27 104553" src="https://github.com/user-attachments/assets/59fde804-ebe0-47d4-b087-c6cceee4d2f1" />

I chose a Port rule, set the protocol to TCP, and specified 23 as the port number.

<img width="1408" height="871" alt="Screenshot 2025-09-27 104758" src="https://github.com/user-attachments/assets/5eb88c59-a418-4a6f-a99d-de1a25ed98b3" />


For the action, I selected Block the connection

I gave the rule a name like __"Block  Telnet"__

<img width="1226" height="822" alt="Screenshot 2025-09-27 105211" src="https://github.com/user-attachments/assets/5f4c5eca-9444-4b44-8110-a92fd1a0cc05" />


 **Test the rule by attempting to connect to that port locally or remotely:**

  *To test locally:* Open a command prompt or PowerShell.
  
  After creating the rule, missing a feature. I went to the Control Panel, clicked on "Programs," and then "Turn Windows features on or off." From there, I enabled the Telnet Client feature.
  I returned to CMD and PowerShell to test the connection again with the same commands as before. My tests still showed that the connection had failed.
  
<img width="1244" height="725" alt="image" src="https://github.com/user-attachments/assets/df9ec752-69d6-4ce3-9a25-ffb1c66611cc" />


#commands

for cmd:
 __telnet localhost 23__
 
 <img width="1476" height="697" alt="Screenshot 2025-09-27 134739" src="https://github.com/user-attachments/assets/c8d263a3-8fad-4241-a57d-4b8350935bf0" />

 
for powershell:

__Test-NetConnection -ComputerName localhost -Port 23__

<img width="1459" height="677" alt="Screenshot 2025-09-27 110246" src="https://github.com/user-attachments/assets/5673969f-bda1-4a4a-9dbf-c03aaf6c653e" />

*To test remotely on ubuntu:*

truly solve the problem, I needed to perform a remote test. I went to my Ubuntu virtual machine to test the connection to my Windows machine.

The Remote Test:

I opened a terminal on my Ubuntu VM and ran the command I had used previously in my Windows CMD, which was:

#command:__telnet <my_windows_ip> 23__

<img width="1898" height="737" alt="Pasted image (3)" src="https://github.com/user-attachments/assets/2719d64e-e208-4430-9d45-ebe74e81b86c" />

**Why it Worked (and still failed): This test was the key to solving my problem. It showed that my connection was failing for a deeper reason than just the firewall. I learned that for a connection to be successful, a service must be running on the port to listen for the connection. My test showed that even though I had tried to fix it, there was no service on my Windows machine to receive the connection.**

 *Remove the test block rule to restore original state*

  * Go back to the "Inbound Rules" list in Windows Firewall.
  * Find the rule you created, "Block Telnet".
  * Right-click on the rule and select *Delete*.
  * A confirmation box will appear. Click *Yes*.
<img width="1685" height="866" alt="Screenshot 2025-09-27 110905" src="https://github.com/user-attachments/assets/e9d72cfa-de4e-484c-a7f6-481383ddfd86" />

   *going back to controll panel removing telnet client to restore original state*

   <img width="547" height="502" alt="image" src="https://github.com/user-attachments/assets/ed666a3b-a18d-499a-9294-eaf0286f7d6a" />




