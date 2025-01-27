---
title: "Identifying outbound IP addresses of a Azure Function"
date: "2024-05-01"
tags: 
  - "analytics"
  - "coding"
  - "data-engineering"
---

Here is [Microsoft's documentation page on the topic](https://learn.microsoft.com/en-us/azure/azure-functions/ip-addresses?tabs=portal). The only reason I am documenting this is because the Azure Resource Explorer failed to load consistently across devices for me. Some tutorials I found, like [this one](https://rubberduckdev.com/app-outbound-ip/), seem to be outdated and the outbound IP addresses weren't listed in any Properties menu in the Function App in the Azure Portal. The Azure CLI method was what worked for me:

```
az functionapp show --resource-group <GROUP_NAME> --name <APP_NAME> --query outboundIpAddresses --output tsv
az functionapp show --resource-group <GROUP_NAME> --name <APP_NAME> --query possibleOutboundIpAddresses --output tsv
```

I wanted this as I was hoping to whitelist the IP addresses in the database firewall to allow for scheduling some SQL scripts to run daily. The issue with this is the following, copied from the above linked Microsoft documentation: "because of autoscaling behaviors, the outbound IP can change at any time when running on a [Consumption plan](https://learn.microsoft.com/en-us/azure/azure-functions/consumption-plan) or in a [Premium plan](https://learn.microsoft.com/en-us/azure/azure-functions/functions-premium-plan)." While at least a few people say [whitelist these IPs anyway](https://stackoverflow.com/questions/49109310/how-to-allow-azure-function-to-access-database-hosted-on-azure-vm), the recommended course of action is to set up a virtual network within Azure.

GitHub Actions also does [not support static IP addresses](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners#larger-runners) through the general free hosted option it looks like - while I can get the IP addresses they use, it is [against best practices to whitelist these](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners#ip-addresses), and they are updated weekly.

How permissions work for a hosted tool is a consistent aspect I need to consider when evaluating new solutions:

- How does the hosted tool get permission to access a private database?

- What adapters does it support (either officially or community driven)?

- Would it rely on direct queries to the database or through a REST API?

- Where do I store the environment secrets in the tool? How is that secured? Is this a paid feature? What does the process look like to update or rotate these secrets?

- How do I test the tool locally using the correct credentials? For example, with an Azure Function, I can use a `local.settings.json` file to include environment variables that will be accessible from the App Settings in the Function App.

- Is there a set of outbound IP addresses that the tool will try to access our data from that we can whitelist, or do we need a more sophisticated approach?

- How does any regulation around data sharing work here, e.g., if there is PII or PHI data, are there issues with which tools pull data and how they connect?
