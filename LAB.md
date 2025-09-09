# Splunk Lab for Phishing Campaigns

This repository documents my cybersecurity home lab designed to detect and analyze phishing campaigns using Splunk and Snort. The lab demonstrates how to:

Ingest labeled email datasets into Splunk to track phishing campaigns.

Analyze user behavior, such as clicks on phishing links, and extract email header information for threat intelligence.

Integrate Snort IDS with Splunk to simulate interactions with phishing emails and monitor suspicious activity in real time.


## Installing Splunk Enterprise and Splunk UniversalForwarder

To begin, I installed the latest versions of both Splunk Enterprise and their Universal Forwader. I made sure to download the 64-bit Windows package for both.


![Splunk Installation](images/splunk_download.png)
![SplunkUF Installation](images/splunkUF_download.png)

## Configuring the Forwader

After running the installer and choosing the default install path, I now need to configure the forwarder to recieve and deploy on my lab's server IP. Thankfully, Splunk makes this easy by just filling in the host and already having the port numbers available.

![Deployment Server](images/configuring_forwarder.png)
![Recieving Indexer](images/configuring_indexer.png)

## PHISHING DATASET INGESTION: CEAS_08.csv & TREC_06.csv 

I found these two datasets online that had **TONS** of phishing emails and decided to use them. I downloaded them and quickly needed to make the Universal Forwarder monitor these filse specifically so they could be ingested into Splunk. 

![Phishing Data](images/ceas08.png)

![Phishing Data](images/trec_06.png)

## Inputs.conf & Outputs.conf

To ensure the phishing dataset is ingested correctly, I configured two main files in the Universal Forwarder: inputs.conf and outputs.conf. In the inputs CONF file, I specificed that the forwarder should monitor the CEAS08 CSV file. I also set the index to phishing so that all records would be stored in the correct location within Splunk. I made sure to define the source type as csv so the data would be parsed appropriately.

![Inputs](images/inputs.png)

In outputs.conf, I configured where the monitored data should go. This involved specifying the recieving Splunk Enterprise instance and the port **127.0.0.1:9997** to forward the logs. By setting up both of these files, the Universal Forwarder monitors the phishing CSV and reliably forwards the data to my Splunk server.

![Outputs](images/outputs.png)

## Bringing the Data to Splunk

After refreshing the UniversalForwader, I finally can now go to my Splunk Enterprise server. Signed in using the username and password that I created during the installation process.

![Sign In](images/sign_in.png)

Once signed in, I need to make it to where Splunk is recieving data from the same port number as my indexer **9997**. To do this, I went Settings --> Forwarding and Receiving --> Configure recieving and added 9997. 

![Port](images/receiving_port.png)

Now, all I have to do is create the index I mentioned in my **inputs.conf** file. To do this, I went to Settings --> Indexes --> New Index and named it **phishing** while keeping everything else default.

![Index](images/index.png)

I can now search through the ingested logs by typing index=phishing into the Search in **Search and Reporting** and set the time to **All Time** to ensure all logs are shown. After a few minutes, we are left with over **55000** events! 

![Search Results](images/searchresults.png)

## Reviewing Logs & Extract Email Headers for Threat Intelligence

With all of these events, I need to create a query that can simplify the logs into meaningful threat intelligence. Since the dataset already provides fields like sender, receiver, subject, date, URLs, and labels, I can treat them like real email headers and extract useful insights. For example, analyzing the sender field highlights which accounts or domains are most commonly associated with phishing attempts. The first was making the label = 1, 1 meaning that the email is malicious.

![Label query](images/label.png)

This simple query managed to cut our events in half and shows the true amount of 25,462 **phishing** emails. I can now dive into certain emails and investigate for each phishing campaign. 

## Log #1

![Phishing Example](images/phishinglog.png)

In this log, we can see the sender uses what looks to be a random email **rookcuduq@yahoo com** stating that ROLEX's are going for $219 *yeah ok*. In the body, they go on to add every luxury watch name and put a very suspicious link at the end that looks like random letters again. 

## Log #2

![Phishing Example2](images/phishinglog2.png)

Here is another example of a new phishing campaign trying to manipulate users through cheap website development. The body has a bunch of fake prices and than drops a very suspicious link that looks like gibberish **http:/3arsqx949s9mllg83lyq3ll.tertialmb com**. Both of these examples seem to target college students due to the **edu** at the receiver's email. 

## Snort IDS & Analyzing User Behavior







