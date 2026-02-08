# End-to-end MLOps with Azure Cloud, using Azure MLOps V2 architecture
1. An Azure subscription. If you don't have an Azure subscription, create a free account before you begin. Try the free or paid version of Azure Machine Learning.
2. An Azure Machine Learning workspace.
3. Git running on your local machine.
4. An organization in Azure DevOps.
5. Azure DevOps project that will host the source repositories and pipelines.
6. The Terraform extension for Azure DevOps if you're using Azure DevOps + Terraform to spin up infrastructure

## Set up authentication with Azure and DevOps
### Create service principal
1. Launch the Azure Cloud Shell.

2. If prompted, choose Bash as the environment used in the Cloud Shell. You can also change environments in the drop-down on the top navigation bar

![image](https://github.com/user-attachments/assets/b249951c-e9dd-49de-98f9-f7fe778f91ca)

3. Copy the following bash commands to your computer and update the projectName, subscriptionId, and environment variables with the values for your project. If you're creating both a Dev and Prod environment, you'll need to run this script once for each environment, creating a service principal for each. This command will also grant the Contributor role to the service principal in the subscription provided. This is required for Azure DevOps to properly use resources in that subscription.
```
projectName="<your project name>"
roleName="Contributor"
subscriptionId="<subscription Id>"
environment="<Dev|Prod>" #First letter should be capitalized
servicePrincipalName="Azure-ARM-${environment}-${projectName}"
# Verify the ID of the active subscription
echo "Using subscription ID $subscriptionID"
echo "Creating SP for RBAC with name $servicePrincipalName, with role $roleName and in scopes     /subscriptions/$subscriptionId"
az ad sp create-for-rbac --name $servicePrincipalName --role $roleName --scopes /subscriptions/$subscriptionId
echo "Please ensure that the information created here is properly save for future use."
```
4. Copy your edited commands into the Azure Shell and run them (Ctrl + Shift + v).
5. After running these commands, you'll be presented with information related to the service principal. Save this information to a safe location, it will be use later in the demo to configure Azure DevOps.
```
{
   "appId": "<application id>",
   "displayName": "Azure-ARM-dev-Sample_Project_Name",
   "password": "<password>",
   "tenant": "<tenant id>"
}
```
6. Repeat Step 3 if you're creating service principals for Dev and Prod environments. For this demo, we'll be creating only one environment, which is Prod.
7. Close the Cloud Shell once the service principals are created

## Set up Azure DevOps
1. Navigate to Azure DevOps.
2. Select create a new project (Name the project `mlopsv2 ` for this tutorial).
   
![image](https://github.com/user-attachments/assets/ffc9669d-019b-4ccc-bfbb-8fb6e32eeb7b)

3. In the project under Project Settings (at the bottom left of the project page) select Service Connections.
4. Select Create Service Connection.

![image](https://github.com/user-attachments/assets/522c1c9d-d364-4d85-a717-7c3c6e57ccc7)

5.Select Azure Resource Manager, select Next, select Service principal (manual), select Next and select the Scope Level Subscription.
  1. Subscription Name - Use the name of the subscription where your service principal is stored.
  2. Subscription Id - Use the `subscriptionId` you used in Step 1 input as the Subscription ID
  3. Service Principal Id - Use the `appId` from Step 1 output as the Service Principal ID
  4. Service principal key - Use the `password` from Step 1 output as the Service Principal Key
  5. Tenant ID - Use the `tenant` from Step 1 output as the Tenant ID
6. Name the service connection ** Azure-ARM-Prod.**

7. Select Grant access permission to all pipelines, then select Verify and Save.

## Set up source repository with Azure DevOps
1. Open the project you created in Azure DevOps
2. Open the Repos section and select Import Repository

![image](https://github.com/user-attachments/assets/781966ec-5596-4b44-91da-1f6fc54c1a84)

3. Enter your repo URL into the Clone URL field. Select import at the bottom of the page
   
![image](https://github.com/user-attachments/assets/db158eec-e1b6-4e6f-8312-e08757b1c3f5)

4. Open the Project settings at the bottom of the left hand navigation pane

5. Under the Repos section, select Repositories. Select the repository you created in previous step Select the Security tab

6. Under the User permissions section, select the mlopsv2 Build Service user. Change the permission Contribute permission to Allow and the Create branch permission to Allow.

![image](https://github.com/user-attachments/assets/d7fc9726-6539-4bbc-b8e9-9c21c04eff89)

7. Open the Pipelines section in the left hand navigation pane and select on the 3 vertical dots next to the Create Pipelines button. Select Manage Security

![image](https://github.com/user-attachments/assets/f8b0c75b-d4f3-4da4-a705-5d6cb6e53e72)

8. Select the mlopsv2 Build Service account for your project under the Users section. Change the permission Edit build pipeline to Allow

![image](https://github.com/user-attachments/assets/60c8b661-924b-4a48-a5ed-efcce665cacd)

## Deploying infrastructure via Azure DevOps
## Training and Deployment Scenario
## Deploying model training pipeline
## Deploying the Trained model
## Clean up resources








