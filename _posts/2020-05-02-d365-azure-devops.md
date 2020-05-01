---
layout: post
title:  Setting up TFS/Azure DevOps for Dynamics 365 F&O
categories: [d365,azure devops]
published: true
---

In this tutorial, I will show you how to set up your D365 dev environment to connect to your Azure DevOps project. I will use our current setup as an example.

### Prerequisites
1. User must atleast have a **Basic** access level on their Azure DevOps organization to access Azure Repos. See this [link](https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/) for the pricing.

2. Rename virtual machine. Name must be unique across your team.
![Rename virtual machine](https://thepracticaldev.s3.amazonaws.com/i/mq8qj50ght5fq22atlgw.PNG)

According to the docs:
> A local development (VHD) environment must be renamed for the following scenarios:

> Accessing a single Microsoft Azure DevOps project across multiple machines: 
*In development topologies, multiple virtual machines (VMs) can't access the same Azure DevOps project if they have the same machine name. Azure DevOps uses the machine name for identification. If you're developing on local VMs that were downloaded from Microsoft Dynamics Lifecycle Services (LCS), you might encounter issues.*

### Setting up the source control

#### .tfignore 

First is to create a .tfignore file to define which files to ignore. Please note that this is not that reliable. Just think of it as your first line of defense. 
The only thing we need to include in the version control is the model descriptor and the xml files located in the model folder.

Here's a sample .tfignore file:
*If you have any suggestions regarding the tfignore file, please let me know.*
```
/MyModel/bin/
/MyModel/XppMetadata/
/MyModel/Resources/
/MyModel/*.txt
/MyModel/*.xml

!/MyModel/Descriptor/*.xml
!/MyModel/MyModel/*.xml

!.tfignore

```

#### Tree structure
Here's how our tree structure would look like:

```
$/Project
+---Trunk
    +---Main
        +---Metadata
        |   +---MyModel
        |      +---Descriptor
        |      |   	MyModel.xml
	|      |
	|      \---MyModel
	|          +---AxClass
	|              MyClass.xml
        \---Projects


```

All other files and folders should be ignored or excluded.



#### Visual Studio

Next step is to configure our Visual studio to connect to Team Foundation. 
To do this, you must log in to the account connected with Azure DevOps. 
Then go to `Team explorer > Manage connections > Connect to Team Project.`
On the form, select your project from the drop down. 
If not found, you may add it manually thru the **Servers** button. 

Please note that you should use the old format of the url which is the `organization.visualstudio.com` instead of the `dev.azure.com/organization/project`

Select the Team project and then click **Connect**.


#### Workspaces

Now that we know how our structure should look like, we need to map the workspaces to link our local files to the source control. 

Your `Project\Trunk\Branch\Metadata\` should be mapped to your local `PackagesLocalDirectory` path.


### To be continued...
