---
author:
  name: Linode
  email: docs@linode.com
description: How to retain Disks with the Linode Images service
keywords: 'linode Images, imagize'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
alias: ['linode-images/']
modified: Tuesday, June 2nd, 2015
modified_by:
  name: Linode
published: 'Thursday, September 25th, 2014'
title: Linode Images
---

*Linode Images* allows you to take snapshots of your disks, and then deploy them to any Linode under your account. This can be useful for bootstrapping a master image for a large deployment, or retaining a disk for a configuration that you may not need running, but wish to return to in the future. Linode Images will be retained whether or not you have an active Linode on your account, which also makes them useful for long term storage of a private template that you may need in the future. There is no additional charge to store images for Linode users, with a limit of 2GB per Image, with a total of 10GB total Image storage and 100 Images per account.

{: .note}
>
>When saving a Linode image it is the aspects of the Linode that are on the **disk** that are saved, not any additional aspects such as IP addresses, fully qualified domain names, and MAC addresses.

## Capturing Your Image

Linode Images captures an exact copy of your disk at the moment when the process is kicked off. Here's how to capture your first image.

 {: .note }
 >While an image can be captured while your Linode is running, we would recommend shutting your Linode down first if you are running any active databases. Capturing an image that includes a running database can cause corruption or data loss in the imaged copy of the database.

1.  Navigate to your Linode Dashboard and select the disk that you wish to freeze from the **Disks** list.

2.  On the **Edit Disk** page, click the **Imagize Image** button.

	[![Edit Disk](/docs/assets/edit-disk-image.png)](/docs/assets/edit-disk-image.png)

3.  On the Image page, you can provide a label for your image. You can also write short notes about the image being captured in the Description box.

	[![Imagize Image](/docs/assets/imagize-image.png)](/docs/assets/imagize-image.png)

4.  Once you click the Imagize button, your image will be frozen for later use. You can view the progress on the Dashboard under the Host Job Queue.

	[![Host Job Queue](/docs/assets/host-job-queue.png)](/docs/assets/host-job-queue.png)

 {: .note}
>Linode Images are limited to 2048MB of data per disk.  You will need to ensure that that data within your disk does not exceed this size limit.

Once the job in the host queue has completed, your Linode's disk has been captured and stored.

## Managing Your Images

The images captured from your Linodes are stored for future usage, and can be modified or removed at any time.  The following instructions will explain how to manage your Linode Images once they have been captured.

1.  Click on the Linodes tab.

2.  Select **Manage Images** at the bottom of the page.

3.  From this page, you can delete existing disks, and view your images quota usage.

4.  You can edit your captured disks by clicking the **Edit** button to the right of the image in question.  This will allow you to edit the image name and notes.

4.  To delete a previously captured disk, simply click the **Delete** link to the right of the image in question.  You will be asked for confirmation prior to the image being deleted.

	[![Images List](/docs/assets/images-list.png)](/docs/assets/images-list.png)

## Deploy From A Saved Image

Deploying one of your saved images to any Linode under your account is a simple process.

1.  Navigate to the Dashboard of the Linode you wish to deploy your image to.

2.  Click the **Deploy a Linux Distribution** link and select your image from the drop down list, under the **Images** heading.

	[![Deploy a Distribution](/docs/assets/deploy-a-distro.png)](/docs/assets/deploy-a-distro.png)

3.  Select your desired disk size and set your root password, then click Deploy to create a configuration profile with your saved disk.

Once you've completed these steps, your saved image will be deployed on your new Linode.
