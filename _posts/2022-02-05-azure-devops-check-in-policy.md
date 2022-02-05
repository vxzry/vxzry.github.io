---
layout: post
title:  Azure DevOps Check-in Policies
categories: [d365,azure devops,tfvc]
published: true
---

Common Check-in policies on TFVC / Azure DevOps.

### Contents
- [Require associated work items on Check-in](#Require-associated-work-items-on-Check-in)
- [Require comments on Check-in](#Require-comments-on-Check-in)


## Require associated work items on Check-in

To require associated work items on each check-in, 

1. On Visual Studio, open **Team Explorer**.

2. Go to Project > Settings > Source Control.

    ![Settings](https://user-images.githubusercontent.com/20976789/152632871-4d224925-4b86-4434-935c-335bc58b26bb.png)

    ![SourceControl](https://user-images.githubusercontent.com/20976789/152632903-ac2f7038-dff8-4efb-af05-0271540a3fe3.png)

3. Go to the **Check-in Policy** tab and click **Add**.

    ![CheckInPolicy](https://user-images.githubusercontent.com/20976789/152632834-cc22ddf1-5225-4a46-b95c-bf619b60437e.png)

4. Select Work Items.

    ![WorkItems](https://user-images.githubusercontent.com/20976789/152632827-f137953c-4ed5-4dec-a3e8-6f8e868cca14.png)


5. Click Ok.

6. Users will see a list of Policy warnings on Pending changes.

    ![RequiredWorkItem](https://user-images.githubusercontent.com/20976789/152633160-2f18e089-9660-4e0d-9d58-4fe844ca9fde.png)

7. Upon clicking **Check In**, they will receive the following error message:

    ![RequiredError](https://user-images.githubusercontent.com/20976789/152633217-64647aba-4d17-4554-88dd-639d69b012a4.png)



## Require comments on Check-in

To require comments on each check-in, 

1. On Visual Studio, open **Team Explorer**.

2. Go to Project > Settings > Source Control.

    ![Settings](https://user-images.githubusercontent.com/20976789/152632871-4d224925-4b86-4434-935c-335bc58b26bb.png)

    ![SourceControl](https://user-images.githubusercontent.com/20976789/152632903-ac2f7038-dff8-4efb-af05-0271540a3fe3.png)

3. Go to the **Check-in Policy** tab and click **Add**.

    ![CheckInPolicy](https://user-images.githubusercontent.com/20976789/152632834-cc22ddf1-5225-4a46-b95c-bf619b60437e.png)

4. Select **Changeset Comments Policy**

    ![ChangesetComment](https://user-images.githubusercontent.com/20976789/152632840-046109c6-bf68-4e02-8078-4fe912e7bc49.png)


5. Click Ok.

6. Users will see a list of Policy warnings on Pending changes.

    ![RequiredComment](https://user-images.githubusercontent.com/20976789/152633175-b8615eeb-c8dc-4e4a-a1e5-8d074e9e28e9.png)

7. Upon clicking **Check In**, they will receive the following error message:

    ![RequiredError](https://user-images.githubusercontent.com/20976789/152633217-64647aba-4d17-4554-88dd-639d69b012a4.png)

