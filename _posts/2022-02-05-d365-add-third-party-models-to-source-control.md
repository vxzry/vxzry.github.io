---
layout: post
title:  Add third-party models to source control for Dynamics 365 F&O
categories: [d365,azure devops,third-party,tfvc]
published: false
---

Add third-party models to source control from deployable package.

### Contents
- [Adding Third-party Deployable Package to Source Control](#adding-third-party-Deployable-Package-to-source-control)
- [Updating Third-party Models to Source Control](#Updating-third-party-models-to-source-control)
- [For Other Development Machines](#For-Other-Development-Machines)
  * [Getting Third-party Models from Source Control for the First Time](#Getting-third-party-models-from-Source-Control-for-the-First-Time)
  * [Updating Third-party models from Source Control](#Updating-third-party-models-from-Source-Control)


## Adding Third-party Deployable Package to Source Control

To add third-party models to the source control (TFVC) from the deployable package given by the provider,

1. On your development machine, install the deployable package by following this guide - [Install an application (AOT) deployable package on a development environment](https://docs.microsoft.com/en-us/dynamics365/fin-ops-core/dev-itpro/deployment/install-deployable-package)

2. After the installation, open Visual Studio as administrator.

3. Go to Dynamics 365 menu > Model Management > Refresh Models.

    ![RefreshModels](https://user-images.githubusercontent.com/20976789/152630573-566675bc-9069-433e-9346-6ee621f5179c.png)

4. To confirm that you have installed the model, go to the Application explorer and type in: `model:<ModelName>`. The installed objects should be listed in italics, which means you don't have access to the metadata.

    ![AOTModelSearch](https://user-images.githubusercontent.com/20976789/152630889-47388bea-037d-4b5d-95ac-64de9a210bb2.png)

5. Open **Source Control Explorer** by clicking View > Other Windows > Source Control Explorer. Or through the Team Explorer window > Project > Source Control Explorer.

    ![SourceControlExplorer](https://user-images.githubusercontent.com/20976789/152631029-11f26a71-f489-436d-9d17-416ca8acf644.png)

6. Navigate to the metadata folder that is mapped on your development machine, such as `MyProject/Trunk/Development/Metadata`. 

7. Right-click the Metadata folder, and then click **New Folder**. Create a folder for the third-party Model. 

    ![NewFolder](https://user-images.githubusercontent.com/20976789/152631103-f23274ca-2037-4d56-8bb0-ef83d7b6c390.png)

8. **Check in** the new folder created.

9. Update workspace mapping, map the new folder from source control to the metadata folder of the third-party Model. 

    From: `$/Project/Development/Metadata/ISVModel`

    To: `C:\AOSService\PackagesLocalDirectory\ISVModel`


10. Select **No** on the Workspace Modified prompt. 

11. On the Source Control Explorer, right-click the created folder, and then click Add Items to Folder. 

    ![AddItems](https://user-images.githubusercontent.com/20976789/152631209-12ff6cb6-422f-40fb-bda8-2e8a03212cbd.png)

12. In the Add to Source Control dialog box, select all (Ctrl+A) the folders except XppMetadata and Descriptor, if they exist, and then click Next. 

    ![AddItems2](https://user-images.githubusercontent.com/20976789/152631282-4e8d6ef7-2497-4437-a6ad-ab3a1a89c192.png)

13. On the next page, on the Excluded items tab, select all files by clicking one of the files and then pressing Ctrl+A. At the bottom of the selection window, click Include item(s).

    ![ExcludedItems](https://user-images.githubusercontent.com/20976789/152631390-3de46acf-8e43-4cb4-aab0-434b9f8a6f13.png)

14. Click Finish.

15. Open the Pending Changes window from the Team Explorer pane or by clicking View > Other Windows > Pending Changes. 

16. Review the changes, enter a check-in comment, and then click Check In.     

---

## Updating Third-party Models to Source Control

Upon receiving an updated package from your providers, 

1. On your development machine, stop IIS, and the Microsoft Dynamics 365 Unified Operations: Batch Management Service.

2. Delete the third-party model folder from `PackagesLocalDirectory`.

3. Install the package. (See step 1 above)

4. Start IIS and Microsoft Dynamics 365 Unified Operations: Batch Management Service.

5. Add all items to source control, including Excluded Items. (See steps 12 & 13 above)

6. Open the Pending Changes window from the Team Explorer pane or by clicking View > Other Windows > Pending Changes. 

7. Review the changes, enter a check-in comment, and then click Check In.   

---

# For Other Development Machines

## Getting Third-party Models from Source Control for the First Time

Once the third-party model is available on the source control, other developers would only need to get the latest version to install the model on their machines.

1. On your deveopment machine, open Visual Studio.

2. Open Source Control Explorer. 

3. Update workspace mapping, go to `PackagesLocalDirectory` and create a new folder for the third-party model.

    ![NewFolder](https://user-images.githubusercontent.com/20976789/152631649-c8289d0b-f623-45ab-be44-8952a7aecc1b.png)


4. Map the third-party model from source control to the new folder you have created. 

    From: `$/Project/Development/Metadata/ISVModel`

    To: `C:\AOSService\PackagesLocalDirectory\ISVModel`

5. Select **Yes** on the Workspace Modified prompt to get the latest version.

6. Build and Sync.


## Updating Third-party models from Source Control

Once the third-party model is updated on the source control, 

1. On your development machine, stop IIS, and the Microsoft Dynamics 365 Unified Operations: Batch Management Service.

2. Delete third-party model folder from `PackagesLocalDirectory`.

3. Open Visual Studio.

4. Open Source Control Explorer and click get latest version.

    ![GetLatest](https://user-images.githubusercontent.com/20976789/152632147-860dc76d-c904-452d-b6a0-e9ab93bfcf60.png)

5. Build and Sync.

6. Start IIS and Microsoft Dynamics 365 Unified Operations: Batch Management Service.



