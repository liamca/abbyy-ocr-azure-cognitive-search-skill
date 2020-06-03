# ABBYY OCR Custom Skill for Azure Cognitive Search

This is a custom skill for Azure Cognitive Search that leverages [ABBYY Cloud OCR](https://www.ocrsdk.com/) to extract text from images.  It is code that leverages Azure Functions to receive input from Azure Cognitive Search to take an image which is passed to ABBYY OCR and returns text back to Azure Cognitive Search.

## Requirements
* Azure Cognitive Search Service: PLease ensure you know how to setup and configure an [Azure Cognitive Search web api based custom skill](https://docs.microsoft.com/en-us/azure/search/cognitive-search-custom-skill-web-api)
* ABBYY Cloud OCR SDK: Register for [ABBYY Cloud OCR SDK](https://www.ocrsdk.com/) account, which comes with 500 free calls.  Once registered, you will need to create an OCR application and ensure you have an Application ID, Password, and ServiceUrl such as https://cloud-westus.ocrsdk.com.
* Postman:  We will use [Postman](https://www.postman.com/downloads/) to test the Custom Skill
* Azure Cognitive Search Power Skills: Download or Clone the [Power Skills](https://github.com/Azure-Samples/azure-search-power-skills) repo 

# Configuration

Go to the location where you downloaded the Azure Cognitive Search Power Skills and find the Text directory.  Within this directory, copy the ABBYYOCR directory from this repo.

Open PowerSkills.sln in the root directory of this Power Skills directory in Visual Studio.  

In the Solution Explorer, Right Click on Text and Choose Add -> Existing Project.

Locate and choose AbbyyOCR.csproj within the \Text\ABBYOCR directory and add it.  

Open AbbyyOCR.cs and update the following lines:

```code
        private const string ApplicationId = @"[Enter ABBYY Application ID]";
        private const string Password = @"[Enter ABBYY OCR Password]";
        private const string ServiceUrl = "[Enter ABBYY Service URL such as https://cloud-westus.ocrsdk.com]";
```

This code assumes that the images will be coming in, in either English, Arabic or Hebrew.  If you wish to change this, update the below code in the ProcessImageAsync function. You can find the list of [supported languages here](https://www.ocrsdk.com/documentation/specifications/recognition-languages/).

```code
var imageParams = new ImageProcessingParams
{
     ExportFormats = new[] { ExportFormat.Docx, ExportFormat.Txt, },
     Language = "English,Arabic,Hebrew",
};
```


# Test

At this point we can test this function.  Click on ABBYYOCR in the Solution Explorer and choose F5.


# Deploy

At this point you can deploy this code to Azure Functions by right clicking on the ABBYYOCR project in the Solution Explorer and choosing Publish. Once it is running you will see a URL such as http://localhost:7071/api/AbbyyOCR.

Open Postman and create a POST request to this URL.

In the Body of the request, change the type to raw and enter JSON in a format such as this the below JSON.

NOTE: For your test, you will need to not only have the URL references to the Blob image which is placed in the formURL field, but also create a SaS Token for this file which is located in the formSasToken field.

```json
{
  "values": 
  [
     {
      "recordId": "foo1",
      "data": { 
             "formUrl":  "https://azsdemos.blob.core.windows.net/blob-files/image1.JPG",
             "formSasToken":  "?sv=..."
      }
    },
         {
      "recordId": "foo2",
      "data": { 
             "formUrl":  "https://azsdemos.blob.core.windows.net/blob-files/image2.JPG",
             "formSasToken":  "?sv=..."
      }
    }
  ]
}
```




