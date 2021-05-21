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

                if (isFileMissing)
                {
                    return new BadRequestObjectResult("The following parameters are required: 'file'");
                }

                string filename = formdata["filename"];
                IFormFile file = req.Form.Files["file"];

                string connectionString = GetConnectionString(context);

                BlobStorage blobStorage = new BlobStorage(connectionString, containerName);
                blobStorage.Initialize();

                CloudBlockBlob blob;

                blob = await blobStorage.CreateBlob(file, filename);

                if (blob != null)
                {
                    // Return url of created blob
                    string blobUrl = blob.Uri.ToString();

                    return new OkObjectResult(blobUrl);
                }
                else
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
                if (String.IsNullOrEmpty(ex.Message))
                {
                    return new BadRequestObjectResult("UnsupportedMediaType. Content type must be form-data");
                }
                else
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

```C#
using System;
using System.IO;
using System.Threading.Tasks;
using System.Linq;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;

using System.Collections.Generic;
using System.Net;

namespace Blobs.Logic
{
    class BlobStorage
    {
        CloudStorageAccount StorageAccount { 
            get {
                if(!String.IsNullOrEmpty(ConnectionString)) {
                    return CloudStorageAccount.Parse(ConnectionString);
                }
                else {
                    return null;
                }
            }  
        }
        CloudBlobClient BlobClient
        {
            get
            {
                if (this.StorageAccount != null)
                {
                    return this.StorageAccount.CreateCloudBlobClient();
                }
                else
                {
                    return null;
                }
            }
        }
        private string ConnectionString;
        private string ContainerName;

        public BlobStorage(string _connectionString, string _containerName)
        {
            if (String.IsNullOrEmpty(_connectionString))
            {
                throw new ArgumentNullException("_connectionString is required");
            } 
            else if (String.IsNullOrEmpty(_containerName))
            {
                throw new ArgumentNullException("_containerName is required");
            }

            this.ConnectionString = _connectionString;
            this.ContainerName = _containerName;
        }

        public void Initialize()
        {
            if (this.StorageAccount == null)
            {
                throw new NullReferenceException("Error accessing Storage Account. Connection string might be invalid.");
            }

            this.CreateContainerIfNotExists();
        }

        CloudBlobContainer Container
        {
            get
            {
                CloudBlobContainer container = null;

                if (this.BlobClient != null && !String.IsNullOrEmpty(this.ContainerName))
                {
                    container = this.BlobClient.GetContainerReference(this.ContainerName);
                }

                return container;
            }
        }

        public async Task<CloudBlockBlob> CreateBlob(IFormFile _file, string _fileName)
        {
            CloudBlobContainer container = this.Container;
            string fileName = _fileName;
            if(container != null)
            {
                if (String.IsNullOrEmpty(Path.GetFileNameWithoutExtension(fileName)))
                {
                    if (String.IsNullOrEmpty(fileName))
                    {
                        fileName = "";
                    }
                    fileName += Guid.NewGuid().ToString();
                }


                string extension = Path.GetExtension(_file.FileName);
                string blobName = $"{fileName}{extension}";

                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

                using (Stream stream = _file.OpenReadStream())
                {
                    try
                    {
                        blob.Properties.ContentType = _file.ContentType;
                        await blob.UploadFromStreamAsync(stream).ConfigureAwait(false);
                    } catch
                    {
                        throw new ArgumentNullException("Failed to upload file, ContentType is not defined.");
                    }
                }

                return blob;
            }

            return null;
        }

        private void CreateContainerIfNotExists()
        {
            CloudStorageAccount storageAccount = this.StorageAccount;
            if(storageAccount != null)
            {
                CloudBlobClient blobClient = this.BlobClient;
                if(blobClient != null)
                {
                    CloudBlobContainer blobContainer = blobClient.GetContainerReference(this.ContainerName);
                    blobContainer.CreateIfNotExistsAsync();

                    // Create a permission policy to set the public access setting for the container. 
                    BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

                    containerPermissions.PublicAccess = BlobContainerPublicAccessType.Blob;

                    //Set the permission policy on the container.
                    blobContainer.SetPermissions(containerPermissions);
                }
            }
        }
    }
}
```

