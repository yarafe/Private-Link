# Private Endpoint integration with FGT

## Introduction

Private endpoint allow you to connect privaely and securely to azure PAAS service such as Azure Storage and Azure SQL database.
Private endpoint will be linked to azure service. It uses interface with private IP address from your Vnet.
We will interoduce here Fortient solution to integrate FGT with private endpoint. 
This will allow FGT to inspect all traffic between client machines and Private endpoint.

## Design

The ARM template will deploy here the following components:
1 Vnet with for subnets:
internal subnet for FGT internal interface 
external subnet for FGT external interface
ProtectedSubnetA where the private endpoint is located
ProtectedSubnetB where the client windows machines can be deployed 
Single FGT Vm with two interfaces which are located in internal and external subnets.
1 public IP address attached to FGT external interface
Private endpoint with interface linked to the selected resource (ex: SQL server)
private DNS zone linked to Vnet
Routing table for ProtectedSubnetA
Routing table for ProtectedSubnetB


![Private_link_integration](images/Azure_Private_link_Deployment_with_FGT.png)

The traffic should be forwarded from client to FGT internal interface for traffice inspection. 
After that FGT will send the traffic from internal interface to the private endpoint interface.

![Traffic_Flow](images/Azure_Private_link_Deployment_with_FGT_Traffic_Flow.png)

Private DNS integration with private endpoint is mandatory here. The clients should be able to resolve the server FQDN with the Private endpoint IP address.

![Private_DNS](images/Private_DNS_Zone.png)

![Private_Endpoint_DNS](images/Private_Endpoint_DNS_Record.png)


You could also verify your private endpoint settings. You can check that the private link resource is linked to the correct service and the connection status is approved.


![Private_Endpoint](images/Private_Endpoint.png)

## Deployment: Azure Portal

Azure Portal Wizard:
[![Azure Portal Wizard](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fyarafe%2FTest%2Fmain%2FmainTemplate.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2Fyarafe%2FTest%2Fmain%2FcreateUiDefinition.json)

Custom Deployment:
[![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2F40net-cloud%2Ffortinet-azure-solutions%2Fmain%2FFortiSandbox%2FAdvance-Deployment%2FmainTemplate.json)
[![Visualize](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true)](http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2F40net-cloud%2Ffortinet-azure-solutions$2Fmain%2FFortiSandbox%2FA-Single-VM%2FmainTemplate.json)

## Support

Fortinet-provided scripts in this and other GitHub projects do not fall under the regular Fortinet technical support scope and are not supported by FortiCare Support Services.
For direct issues, please refer to the [Issues](https://github.com/40net-cloud/fortinet-azure-solutions/issues) tab of this GitHub project.

## License

[License](LICENSE) © Fortinet Technologies. All rights reserved.
