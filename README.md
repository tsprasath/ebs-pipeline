# ebs-pipeline

Setup Elastic Beanstalk Application 

* Setting Up: Create an AWS Account
* Step 1: Create an Example Application
* Step 2: Explore Your Environment
* Step 3: Deploy a New Version of Your Application


# Setting Up: Create an AWS Account
If you're not already an AWS customer, you need to create an AWS account. Signing up enables you to access Elastic Beanstalk and other AWS services that you need.

To sign up for an AWS account

Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk).

Follow the instructions shown.

# Step 1: Create an Application and an Environment

To create your example application, you'll use the **Create a web app** console wizard. It creates an Elastic Beanstalk application and launches an environment within it. An environment is the collection of AWS resources required to run your application code

## To create an example application

Open the Elastic Beanstalk console using this link: https://console.aws.amazon.com/elasticbeanstalk/home#/gettingStarted?applicationName=getting-started-app

for Platform, choose a platform, and then choose Create application.

To run the example application on AWS resources, Elastic Beanstalk takes the following actions. They take about five minutes to complete.

1. Creates an Elastic Beanstalk application named getting-started-app.

1. Launches an environment named GettingStartedApp-env with these AWS resources:

* An Amazon Elastic Compute Cloud (Amazon EC2) instance (virtual machine)
* An Amazon EC2 security group
* An Amazon Simple Storage Service (Amazon S3) bucket
* Amazon CloudWatch alarms
* An AWS CloudFormation stack
* A domain name

For details about these AWS resources, see AWS Resources Created for the Example Application.

![https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-events.png](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-events.png)

# Step 2: Explore Your Environment
To see an overview of your Elastic Beanstalk application's environment, use the environment dashboard in the Elastic Beanstalk console.

To view the environment dashboard

1. Open the Elastic Beanstalk console.

1. Choose GettingStartedApp-env.
   ![https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-chooseenvironment.png](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-chooseenvironment.png)


   The environment dashboard shows top level information about your environment. This includes its URL, its current health status, the name of the currently deployed application version, its five most recent events, and the platform version that the application is running on.

   ![https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-dashboard.png](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-dashboard.png)

   While Elastic Beanstalk creates your AWS resources and launches your application, the environment is in a Pending state. Status messages about launch events are continuously added to the dashboard.

The environment's URL is located in the upper-right corner of the dashboard, next to the Actions menu. This is the URL of the web application that the environment is running. Choose this URL to get to the example application's Congratulations page.

The navigation page on the left side of the console links to other pages that contain more detailed information about your environment and provide access to additional features:

* **Configuration** – Shows the resources provisioned for this environment, such as the Amazon Elastic Compute Cloud (Amazon EC2) instances that host your application. You can configure some of the provisioned resources on this page.

* **Health** –  Shows the status of and detailed health information about the Amazon EC2 instances running your application.

* **Monitoring** – Shows statistics for the environment, such as average latency and CPU utilization. You can use this page to create alarms for the metrics that you are monitoring.

* **Events** – Shows information or error messages from the Elastic Beanstalk service and from other services whose resources this environment uses.

* **Tags** – Shows environment tags and allows you to manage them. Tags are key-value pairs that are applied to your environment.

# Step 3: Deploy a New Version of Your Application

Periodically, you might need to deploy a new version of your application. You can deploy a new version at any time, as long as no other update operations are in progress on your environment.

The application version that you started this tutorial with is called Sample Application.


To update your application version

1. Download the sample application that matches your environment's platform. Use one of the following applications.

1. Open the Elastic Beanstalk console.

2. From the Elastic Beanstalk applications page, choose getting-started-app, and then choose GettingStartedApp-env.

3. In the Overview section, choose Upload and Deploy.

4. Choose Choose File, and then upload the sample application source bundle that you downloaded.
5. Choose Deploy.

![https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-version-upload.png](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-version-upload.png)

# Using Shell script to deploy EBS applicaion from S3 Bucket
```
# AWS CLI Command to copy files from S3.
aws s3 cp myapp.zip s3://myapp2126/aws-s3-ebsdeploy.zip

# AWS CLI Command to create an application on AWS EBS with the required parameters.               
aws elasticbeanstalk create-application-version --application-name my-app --version-label v1 --description myapp --source-bundle S3Bucket="myapp2126",S3Key="aws-s3-ebsdeploy.zip" --auto-create-application

# AWS CLI Command to create an environment n AWS EBS with the required parameters.
aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --solution-stack-name "64bit Amazon Linux 2015.03 v1.4.6 running PHP 5.6"

# Update Environment
aws elasticbeanstalk update-environment --application-name my-app --environment-name my-env --version-label v2

# Check Health Status
aws elasticbeanstalk describe-environments --environment-name my-env 

# Check App Deploy Version

aws elasticbeanstalk describe-environments --application-name my-app --environment-name my-env --version-label v2
