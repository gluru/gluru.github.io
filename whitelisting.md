# White-listing Kare IPs

In order to fetch data from third party platforms Kare uses fetchers
which, in order to keep documents in sync, are scheduled to connect and fetch all new data over time. 
Fetcers requests are routed though a NAT with a fixed IP. 

Certain platforms such as Salesforce Cloud or Zendesk allows enterprise accounts to restrict access 
only to white listed IP addresses. To enable Kare in those environment customers can simply white list 
the NAT IP address as indicated in the followign table.

Environment | IP
Multi-tennancy EU | 104.199.44.45
Multi-tennancy US | 35.188.108.210
Single tennancy| please consult your customer success contact
