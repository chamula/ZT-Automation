# Microsoft Zero Trust
This is an effort to build a custom Azure Policy Initiative incorporating DoD Zero Trust requirements with Azure builtin policy definitions.   

For example, the Microsoft Zero Trust Policy is currently a custom policy initiative assembled using builtin policy definitions applicable to our analysis of Zero Trust.  Microsoft Zero Trust helps organizations align to the Zero Trust requirements and guidance released by many organizations including: 
- Executive Order on Improving the Nation’s Cybersecurity 
- OMB 22-09 Moving the U.S. Government Toward Zero Trust Cybersecurity Principles
- CISA’s Zero Trust Maturity Model
- NIST’s SP 800-207, Zero Trust Architecture
- Department of Defense’s Zero Trust Reference Architecture

It is only meant to test technical requirements AND NOT provide attestations."

## What is Zero Trust?
Zero Trust is the term for an “evolving set of cybersecurity paradigms that move defenses from static, network-based perimeters to focus on users, assets, and resources", according to NIST SP 800-207. At its core, ZT assumes no implicit trust is granted to assets or users based solely on their physical or network location (i.e., local area networks versus the Internet) or asset ownership (enterprise or personally owned). This shift in philosophy is a significant change in legacy authentication and security mechanisms.


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

The `resources` section is used to define the resources that will be deployed by the ARM template. This can include virtual machines, storage accounts, networking resources, and more.  This field will contain the following `parameters`, `policyDefinitionGroups`, and `policyDefinitions` fields as properties.

- `type`: The type of the resource being ingested Ex:"Microsoft.Authorization/policySetDefinitions"
- `apiVersion`: The API version of the resource type
- `name`: The Policy Set Name
- `properties`: Contains the properties of the policy initiative

![image](https://github.com/chamula/ZT-Automation/assets/100379015/c2021ce4-e160-46ae-925d-ec1b71c33ba8)



## Parameters

The "parameters" field contains the parameter definitions for parameters used within the policy. These parameters can be used to customize the behavior of your policies based on the specific needs of your organization. Some examples of parameters that you might define in this section include:

- `effect`: Determines what happens when the policy rule is evaluated to match.
- `location`: The location where your resources will be deployed.
- `environment`: The environment in which your resources will be deployed (e.g. dev, test, prod).
- `resourceGroup`: The name of the resource group in which your resources will be deployed.

## PolicyDefinitionGroups

The "policyDefinitionGroups" section of this template is where you can group together related policy definitions. This can be useful if you have a large number of policies that you want to manage in a more organized way. Some examples of policy definition groups that you might define in this section include:

- `name`: The name of the definition group
- `category`: The name of the category for the definition group.
- `displayName`:  The name displayed in reporting.
- `security`: A group of policies related to security best practices.
- `costManagement`: A group of policies related to cost management and optimization.
- `compliance`: A group of policies related to compliance with industry regulations.

## PolicyDefinitions

The "policyDefinitions" section of this template is where you can define the actual policy definitions that you want to enforce. These definitions can be used to ensure that your resources are deployed and configured in a way that meets your organization's requirements. Some examples of policy definitions that you might define in this section include:

- `policyDefinitionReferenceId`: Audits Linux VMs allowing remote connections from accounts with no passwords.
- `enforce-https`: A policy that requires all web applications to use HTTPS.
- `enforce-tagging`: A policy that requires all resources to be tagged with specific metadata.
- `enforce-backup`: A policy that requires all databases to be backed up on a regular basis.

---
## Questions
### Why rebuild an ARM template instead of modifying the json and reuploading the custom policy initiative?
This question sparked the creation of this repository. The json extracted using the script above does not include the $schema which is required for ingestion into Azure.
