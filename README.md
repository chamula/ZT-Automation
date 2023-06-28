# Microsoft Zero Trust
This is an effort to build a custom Azure Policy Initiative incorporating DoD Zero Trust requirements with Azure builtin policy definitions.   

## Why we need Zero Trust
NIST 800-53, USGov Guidance, 

## Prerequisites 

# Extracting info from prebuilt policies 

```
((Get-AzPolicySetDefinition -Name '<policy ID found at the end of the Assignment ID>') | ConvertTo-Json -Depth 10).Replace('"PolicyType": 2', '"PolicyType": "Builtin"') | Out-File -FilePath C:\Microsoft Zero Trust.json
```
Replace the -Name string with the ID of the policy you want to extract
-Name ex:69edaed0e8e4487687165501

Replace the -FilePath with the desired location of where you want to save the generated json file

---

# How to Build the Template

This template contains five different fields: `$schema`, `resources`, `parameters`, `policyDefinitionGroups`, and `policyDefinitions`. Each section is described in more detail below.
The simplest method is to copy and paste each section from the extracted json file into the corresponding template field.

## $Schema
The `$schema` section is used to define the schema version of the ARM template. This is important because different versions of the schema may have different syntax and features.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "policySetName": "string",
        "policySetDisplayName": "string",
        "policySetDescription": "string"
    },
```

## Resources

The `resources` section is used to define the resources that will be deployed by the ARM template. This can include virtual machines, storage accounts, networking resources, and more.

- `type`:
- `apiVersion`:
- `name`: The Policy Set Name
- `properties`: Contains the properties of the policy initiative

## Parameters

The "parameters" section of this template is where you can define any input parameters that your policy definitions may require. These parameters can be used to customize the behavior of your policies based on the specific needs of your organization. Some examples of parameters that you might define in this section include:

- `location`: The location where your resources will be deployed.
- `environment`: The environment in which your resources will be deployed (e.g. dev, test, prod).
- `resourceGroup`: The name of the resource group in which your resources will be deployed.

## PolicyDefinitionGroups

The "policyDefinitionGroups" section of this template is where you can group together related policy definitions. This can be useful if you have a large number of policies that you want to manage in a more organized way. Some examples of policy definition groups that you might define in this section include:

- `security`: A group of policies related to security best practices.
- `costManagement`: A group of policies related to cost management and optimization.
- `compliance`: A group of policies related to compliance with industry regulations.

## PolicyDefinitions

The "policyDefinitions" section of this template is where you can define the actual policy definitions that you want to enforce. These definitions can be used to ensure that your resources are deployed and configured in a way that meets your organization's requirements. Some examples of policy definitions that you might define in this section include:

- `enforce-https`: A policy that requires all web applications to use HTTPS.
- `enforce-tagging`: A policy that requires all resources to be tagged with specific metadata.
- `enforce-backup`: A policy that requires all databases to be backed up on a regular basis.
