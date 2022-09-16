# Private Endpoint integration with FGT

## Introduction

Private endpoint allow you to connect privately and securely to azure PAAS service such as Azure Storage and Azure SQL database.
Private endpoint will be linked to azure service. It uses interface with private IP address from your Vnet.

We will interoduce here Fortient solution to integrate FGT with private endpoint. 
This will allow FGT to inspect all traffic between client machines and Private endpoint.

You can find more information about private endpoint properties and supported resources from [here](https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview).

## Design

The ARM template will deploy here the following components:

- 1 Vnet with for subnets:
  - Internal subnet for FGT internal interface 
  - External subnet for FGT external interface
  - ProtectedSubnetA where the private endpoint is located
  - ProtectedSubnetB where the client windows machines can be deployed 

- Single FGT Vm with two interfaces which are located in internal and external subnets.
- 1 public IP address attached to FGT external interface
- Private endpoint with interface linked to the selected resource (ex: SQL server)
- Private DNS zone linked to Vnet
- Routing table for ProtectedSubnetA
- Routing table for ProtectedSubnetB


![Private_link_integration](images/Azure_Private_link_Deployment_with_FGT.png)

The traffic should be forwarded from client to FGT internal interface for traffice inspection. 
After that FGT will send the traffic from internal interface to the private endpoint interface.

![Traffic_Flow](images/Azure_Private_link_Deployment_with_FGT_Traffic_Flow.png)

Private DNS integration with private endpoint is mandatory here. The clients should be able to resolve the server FQDN with the Private endpoint IP address.

![Private_DNS](images/Private_DNS_Zone.png)

![Private_Endpoint_DNS](images/Private_Endpoint_DNS_Record.png)

There is a list of DNS zone names related to each resource/sub resource. Microsoft reommend to use that list for DNS zone integrated with the private endpoint.
Please, check the link from [here](https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-dns).

You could also verify your private endpoint settings. You can check that the private link resource is linked to the correct service and the connection status is approved.


![Private_Endpoint](images/Private_Endpoint.png)

When the private endpoint is created , a route will be injected in the routing table to forward the traffic to the end point. We have overwrite this route to forward the traffic from the client to FGT first. You can check The effective routes on the client interface to see the valid routes.

![Effective_Routes_Client](images/Effective_Routes_Client.png)

The screenshots below show how routing table looks like for private endpoint and client subnets. Basically, all traffic should be forwarded to the FGT internal interface behalf the traffic within the same subnet.

![RT_Private_Endpoint](images/RT_Private_Endpoint.png)


![RT_Clients](images/RT_Clients.png)

Microsoft has announced recently about feature enhancement related to UDR support for private endpoint.There is no need now to use/32 prefix for the endpoint. 
you can use wider prefix in the routing table for routes destined to private endpoints. This enhancement requires enable Private endpoints subnet property which is callled PrivateEndpointNetworkPolicies.

![PrivateEndpointNetworkPolicies](images/PrivateEndpointNetworkPolicies.png)


## Deployment: Azure Portal


Custom Deployment:
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fyarafe%2FPrivate-Link%2Fmain%2Fazuredeploy.json)


## Support

Fortinet-provided scripts in this and other GitHub projects do not fall under the regular Fortinet technical support scope and are not supported by FortiCare Support Services.
For direct issues, please refer to the [Issues](https://github.com/40net-cloud/fortinet-azure-solutions/issues) tab of this GitHub project.

## License

[License](LICENSE) © Fortinet Technologies. All rights reserved.
