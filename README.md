# Personalize POC Guide

Amazon Personalize is a machine learning service that allows you to build and scale recommendation/personalization models in a quick and effective manner. The content below is designed to help you build out your first models for your given use case and makes assumptions that your data may not yet be in an ideal format for Amazon Personalize to use.

This repository assumes a base familiarity with the service and if you have not already done so it is recommended that you use the getting-started material below.

## Introduction to Amazon Personalize

If you are not familiar with Amazon Personalize you can learn more about this tool on these pages:

* [Product Page](https://aws.amazon.com/personalize/)
* [GitHub Sample Notebooks](https://github.com/aws-samples/amazon-personalize-samples)
* [Product Docs](https://docs.aws.amazon.com/personalize/latest/dg/what-is-personalize.html)

## Completed Example

The notebooks have been scrubbed of all output before usage, however if you'd like to see a fully worked out example of this process, explore the notebooks in the `completed` folder.

## Process:

1. Deploying Your Working Environment
1. Validating and Importing User-Item-Interaction Data
1. Validating and Importing Item-Metadata
1. Validating and Importing User-Metadata
1. Creating and Evaluating Your First Solutions
1. Importing Item / User metadata to Use
1. Creating and Evaluating Related Metadata Solutions
1. Next Steps

That is a genereal order to this proccess, however if you are operating this as an assisted 2 day on-site POC. It is recommended that you operate steps 2 and 4 beforehand. Once the related data has been successfully imported you can delete it so that it does not muddy the results from your first Predictor.


## Deploying Your Working Environment

As mentioned above, the first step is to deploy a CloudFormation template that will perform much of the initial setup work for you. In another browser window or tab, login to your AWS account. Once you have done that, open the link below in a new tab to start the process of deploying the items you need via CloudFormation.

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=ForecastDemo&templateURL=https://chriskingpartnershare.s3.amazonaws.com/ForecastPOC.yaml)

Follow along with the screenshots below if you have any questions about deploying the stack.

### Cloud Formation Wizard

Start by clicking `Next` at the bottom like this:

![StackWizard](static/imgs/img1.png)

On this page you have a few tasks:

1. Change the Stack name to something relevant like `ForecastPOC`
2. Decide if you want to use an existing VPC or not
3. Change the Notebook Name (Optional)
4. Alter the VolumeSize for the SageMaker EBS volume, default is 10GB, if your dataset is expected to be larger, please increase this accordingly.


When you are done click `Next` at the bottom.

![StackWizard2](static/imgs/img2.png)

This page is a bit longer so scroll to the bottom to click `Next`. All of the defaults should be sufficient to complete the POC, if you have custom requirements alter as necessary.

![StackWizard3](static/imgs/img3.png)


Again scroll to the bottom, check the box to enable the template to create new IAM resources and then click `Create Stack`.

![StackWizard4](static/imgs/img4.png)

For a few minutes CloudFormation will be creating the resources described above on your behalf it will look like this while it is provisioning:

![StackWizard5](static/imgs/img5.png)

Once it has completed you'll see green text like below indicating that the work has been completed:

![StackWizard5](static/imgs/img6.png)

Now that your environment has been created go to the service page for Sageamaker by clicking `Services` in the top of the console and then searching for `SageMaker` and clicking the service.


![StackWizard5](static/imgs/img7.png)

From the SageMaker console scroll until you see the green box indicating now many notebooks you have in service and click that.

![StackWizard5](static/imgs/img8.png)

On this page you will see a list of any SageMaker notebooks you have running, simply click the `Open JupyterLab` link on the Forecast POC notebook you have created

![StackWizard5](static/imgs/img9.png)

This will open the Jupyter environment for your POC, think of it as a web based data science IDE if you are not familiar with it. It should Automatically open the `ForecastPOC` folder for you, but if it does not do that by clicking on the folder icon in the browser on the left side of the screen and follow the documentation below to get started with your POC!



## Validating and Importing User-Item-Interaction Data

The core data for every algorithm supported in Amazon Personalize is User-Item-Interaction data, this notebook will guide you through the process of identifying this data, then formatting it for the service, defining your schema, and lastly importing it. 

Open `Validating_and_Importing_User_Item_Interaction_Data.ipynb` and follow along there.

Once you have completed this, you can move onto the Item and User Metadata sections.

## Validating and Importing Related Time Series Data

Amazon Forecast can certainly generate predictions using only the target data but the real power of the service comes into play when adding related time series information to facilitate better understanding of external signals, as well as item metadata that allows DeepAR+ to make assumptions about how a time series may behave when missing chunks of information.

Open `Validating_and_Importing_Related_Time_Series_Data.ipynb` and follow along there to prepare the dataset for the POC/Amazon Forecast.

## Creating and Evaluating Your First Predictors

In Amazon Forecast a model that has been trained on your data is called a Predictor, the notebook below will guide you through using the data you imported earlier to build your first predictors. At the end there is a bonus bit on running AutoML to determine the best. This is advised to be done before going home for the day as the process will take a number of hours to complete.

Open `Creating_and_Evaluating_Predictors.ipynb` and follow along to build these Predictors and see their results.

## Importing Related Time Series Data

Upon completing the initial models with just the target time series data go back to `Validating_and_Importing_Related_Time_Series_Data.ipynb` and execute the import job again if you deleted it during your validation phase. Once the data has been imported you are ready to move onto the next session of building a model with the related data.

## Creating and Evaluating Related Time Series Enabled Predictors

During this section you'll only be creating new models with Prophet and DeepAR+ this is because they are the only algorithms to incorporate related time series into their forecasts at this time within the service. As existing algorithms are modified or new ones are introduced this section will expand to cover those.

To get started simply open `Creating_and_Evaluating_Related_Time_Predictors.ipynb` this will be the last section of the POC that is guided and the rest will be an exploratory analysis to determine the value of any improvements.

## Next Steps

The next step is to either compare the results from Forecast against a previous approach and determine which one is more performant, if Forecast is more performant or there is no existing system then the path to production is determining how to integrate the results with planners, analysts, or other software solutions. The data again is exportable in JSON or CSV so it is easy to develop automated procedures for this integration.