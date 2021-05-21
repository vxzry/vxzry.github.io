---
layout: post
title: Uploading files to Azure Blob Storage Using Azure Functions
categories: [azure blob storage, azure function]
---

In this guide, we'll be using Azure Functions in C# to upload a file to Azure Blob Storage.

### Prerequisites

- Azure blob storage connection string 
- Packages:
    - Microsoft.Azure.WebJobs
    - Microsoft.Azure.Webjobs.Extensions.Storage
    - Microsoft.NET.Sdk.Functions 

### Azure Function 

Create your Azure Function and use an HTTP Trigger. 

```C#
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Storage.Blob;
using Microsoft.Extensions.Configuration;
using Blobs.Logic;
using Blobs.Entities;

namespace Blobs.Functions
{

    public static class UploadBlob
    {
        [FunctionName("upload")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
            ILogger log, ExecutionContext context)
        {
            try
            {
                var formdata = await req.ReadFormAsync();

                bool isFileMissing = (req.Form.Files.Count == 0 && req.Form.Files["file"] == null);	
				string containerName = "my-container";

                if(isFileUrlMissing && isFileMissing)
                {
                    return new BadRequestObjectResult("The following parameters are required: 'file'");
                }


                IFormFile file = req.Form.Files["file"];

                string connectionString = GetConnectionString(context);

                BlobStorage blobStorage = new BlobStorage(connectionString, containerName);
                blobStorage.Initialize();

                CloudBlockBlob blob;
                
                blob = await blobStorage.CreateBlob(file, filename);

                if(blob != null)
                {
					// Return url of created blob
                    string blobUrl = blob.Uri.ToString();

                    return new OkObjectResult(blobUrl);
                } else
                {
                    return new BadRequestObjectResult("Failed to create blob.");
                }

            }
            catch (ArgumentNullException ex)
            {
                return new UnprocessableEntityObjectResult(ex.Message);
            }
            catch (NullReferenceException ex)
            {
                return new UnprocessableEntityObjectResult(ex.Message);
            }
            catch (Exception ex)
            {
                if(String.IsNullOrEmpty(ex.Message))
                {
                    return new BadRequestObjectResult("UnsupportedMediaType. Content type must be form-data");
                } else
                {
                    return new UnprocessableEntityObjectResult(ex.Message);
                }
            }
        }
        private static string GetConnectionString(ExecutionContext executionContext)
        {
            var config = new ConfigurationBuilder()
                            .SetBasePath(executionContext.FunctionAppDirectory)
                            .AddJsonFile("local.settings.json", true, true)
                            .AddEnvironmentVariables().Build();
            string connectionString = config["AzureWebJobsStorage"];

            return connectionString;
        }
    }
}

```

