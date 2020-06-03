# ABBYY OCR Custom Skill for Azure Cognitive Search

This is a custom skill for Azure Cognitive Search that leverages [ABBYY Cloud OCR](https://www.ocrsdk.com/) to extract text from images.  It is code that leverages Azure Functions to receive input from Azure Cognitive Search to take an image which is passed to ABBYY OCR and returns text back to Azure Cognitive Search.

## Requirements
* Azure Cognitive Search Service: PLease ensure you know how to setup and configure an [Azure Cognitive Search web api based custom skill](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-web-api)
* ABBYY Cloud OCR SDK: Register for [ABBYY Cloud OCR SDK](https://www.ocrsdk.com/) account, which comes with 500 free calls.  Once registered, you will need to create an OCR application and ensure you have an Application ID, Password, and ServiceUrl such as https://cloud-westus.ocrsdk.com.
* Azure Cognitive Search Power Skills: Download or Clone the [Power Skills](https://github.com/Azure-Samples/azure-search-power-skills) repo 

# Configuration

Go to the location where you downloaded the Azure Cognitive Search Power Skills and find the Text directory.  Within this directory, copy the ABBYYOCR directory from this repo.

Open PowerSkills.sln in the root directory of this Power Skills directory in Visual Studio.  

In the Solution Explorer, Right Click on Text and Choose Add -> Existing Project.

Locate and choose AbbyyOCR.csproj within the \Text\ABBYOCR directory and add it.  

Open AbbyyOCR.cs and update the following lines:

<code>
  
        private const string ApplicationId = @"[Enter ABBYY Application ID]";
  
        private const string Password = @"[Enter ABBYY OCR Password]";
        
        private const string ServiceUrl = "[Enter ABBYY Service URL such as https://cloud-westus.ocrsdk.com]";
</code>
# Test

At this point we can test this 


# Deploy

At this point you can deploy this code to Azure Functions by right clicking on the ABBYYOCR project in the Solution Explorer and choosing Publish. 



