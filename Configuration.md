**Configuration**

1. Open Splunk Web at http://localhost:8000 and log in. It should look like sign_in.png located in the repository.


2. Create the Phishing Index. 
Go to Settings → Indexes → New Index

Name the index: phishing

Save the new index.

Configure Receiving on the Indexer

Go to Settings → Forwarding and Receiving → Receive Data → Add New

Select TCP as the data input type.

Enter the port number: 9997

Save changes. This allows Universal Forwarders to send logs to the indexer.
