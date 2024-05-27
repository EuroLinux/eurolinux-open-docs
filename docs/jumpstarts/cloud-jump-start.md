# Eurolinux on clouds Jump Start

This document contains the necessary information to create Your own EuroLinux instance on cloud You prefer.

## Amazon Web Services (AWS)
AWS was launched in 2006 and has since grown to become one of the largest cloud computing platforms in the world, with millions of customers and clients ranging from startups to enterprises.

### How to create EuroLinux instance on AWS
1. Check our products on [AWS Marketplace](https://aws.amazon.com/marketplace/search/results?searchTerms=eurolinux) and select Your favourite.
2. Click "Continue to Subscribe" and then "Continue to Configuration"
3. Select version, region and click "Continue to Launch"
4. Configure the instance as You prefer. Remember to select or create new key pair - it's necessary to create secure connection with instance.
5. Click "Launch"
6. To get the ip of this instance, go to the [EC2 console](https://console.aws.amazon.com/ec2/home) and select "Instances"
7. Click on Instance ID of the newly created instance and copy ip address
8. You can log into instance using the `ssh` comand with `-i key-file.pem` option

## Microsoft Azure
With its strong focus on security and compliance, Azure has become a popular choice for organizations in regulated industries such as healthcare and finance, as well as for government agencies.

### How to create EuroLinux instance on Microsoft Azure
1. On the [Azure Marketplace page](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=eurolinux&page=1), click "Get It Now" and accept the service terms.
2. You will be redirected to the Azure portal, where you will see a "Create" button after logging in.
3. In the "Basics" section of the virtual machine settings menu, pay special attention to the "Administrator account" section, where you will choose the type of authentication and enter a username that you will use to log in to the machine.
4. Completing the creation of a typical virtual machine requires simply filling in the required fields in the "Basics" section. Then move on to "Review + create" and click the "Create" button.
5. Your virtual machine will be created along with all the resources needed for it to function properly. To connect to it, simply select it and use one of the connection options (SSH, RDP, or Bastion) offered by Azure.

## Google Cloud Platform (GCP)
GCP is designed to allow developers and businesses to build, deploy, and run applications and services on Google's infrastructure.

### How to create EuroLinux instance on GCP
1. Make sure you have `gcloud` tool and all its components installed.
2. Log into your GCP account via `gcloud` tool
3. Choose or create a new project.
4. Run the following command in the console:
    ```bash
    gcloud beta compute instances create [instance-name] --zone=[zone-name] --machine-type=[machine-type] --subnet=default --image=[image-url] --boot-disk-size=[disk-size]
    ```

    Where:
    * `[instance-name]` is the desired name for the virtual machine.
    * `[zone-name]` is the zone where the virtual machine will be created.
    * `[machine-type]` is the type of machine to be created.
    * `[image-url]` is the URL of the image.
    * `[disk-size]` is the size of the boot disk.

    For example to create EuroLinux 8.6 instance in a us-central1-a region, type:
    ```bash
    gcloud beta compute instances create eurolinux-server1 --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default --image=https://www.googleapis.com/compute/v1/projects/eurolinux-cloud/global/images/eurolinux-8-6 --boot-disk-size=10GB
    ```

## Alibaba Cloud
Alibaba Cloud, also known as Aliyun aims to provide reliable and secure cloud computing solutions for businesses and organizations around the world, with a focus on serving the Asia-Pacific market.

### How to create EuroLinux instance on Alibaba Cloud
1. Go to [Alibaba Cloud Marketplace](https://marketplace.alibabacloud.com/products?keywords=eurolinux) and select your favorite EuroLinux operating system image.
2. Review the product description and then click the "Choose Your Plan" button. You will now be redirected to the Aliyun console page where you will continue creating the instance.
3. Configure the instance to meet your needs. At this point, you can also change the payment type to subscription.
4. In the System Configurations tab, select or create a "Key Pair" that will allow you to access the machine.
5. Finally, accept the terms and create the instance by clicking "Create Instance". After a few minutes, your instance should be ready to use.

## OpenStack
OpenStack provides a flexible and customizable platform that can be used for a variety of cloud computing needs, from web hosting to big data processing to scientific computing.

**Important:** It is a cloud-generic images.

### How to create EuroLinux instance on OpenStack
1. [Download](https://fbi.cdn.euro-linux.com/images) Your favourite EuroLinux image in qcow2 or raw format. For example: `https://fbi.cdn.euro-linux.com/images/EL-9-cloudgeneric-2023-03-19.qcow2`
2. Login to the OpenStack dashboard.
3. Click on the "Create Image" button in "Compute" -> "Images" section.
4. Fill in the required information and upload this image to OpenStack.
5. To Launch Your instance with image You have just created simply click "Launch Instance" in "Compute" -> "Instances" section and fill the required informations.
6. Make sure You create Your own key-pair and attach it to this instance.
7. To log into Your EuroLinux instance copy the IP address and using the `ssh` comand with `-i key-file.pem` option, login as root to newly created machine.

**Important:** EuroLinux 9 have the root login without-password enabled by default, which means You have to assign the key-pair to Your instance to login.
