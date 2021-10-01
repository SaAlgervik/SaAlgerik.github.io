---
layout: post
title: Files In The Cloud
subtitle: For Better Or For Worse?
cover-img: https://images.unsplash.com/photo-1461360228754-6e81c478b882?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1474&q=80
thumbnail-img: https://images.unsplash.com/photo-1569235186275-626cb53b83ce?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1472&q=80
share-img: /assets/img/path.jpg
tags: [Files]
---

#### Code 
So if you open [this site](https://upploadfiles20211001110816.azurewebsites.net/), you will first get the option to upload you own image, and you'r welcome to do that. When you press "Upload" you can go to "Images" and your image will appear. I have taken alot of time to make the first field a "drag and drop" thing, so you can just take a file, drop it and upload. After way to many hours i realised that it was a bit to much job to do before i hda to sit down and write this. 

_______________________

This diagram is loaded from Azure.

![Image From Blob](https://blobstoragetestazure.blob.core.windows.net/testblobfromappcad5ecc8-9101-4ab2-bf11-e312fb963029/ImagesForBlog/Untitled Diagram(1).png)

________________________


 ```
  public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;
        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }
        [BindProperty]
        public IFormFile Upload { get; set; }   // Saves the image 
        public async Task<IActionResult> OnPostAsync()
        {
            if (Upload == null)
                return Page();

            string connectionString = "Connect_STR";


            BlobServiceClient blobServiceClient = new(connectionString); // Connets to the Blob 

            string containerName = "testblobfromapp0af289d0-1413-46f6-b59d-b7b6cbd434c8"; // Stores container name 

            BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName); // Finds the container
            string fileName = Guid.NewGuid().ToString() + Upload.FileName.ToString();  // Creats a name for the new Image

            BlobClient blobClient = containerClient.GetBlobClient(fileName); 

            using (Stream file = Upload.OpenReadStream()) // Reads the file
            {
                await blobClient.UploadAsync(file); // Uploads the image 
                file.Close();                       // Closes the connection
            }

            return Page();
        } 
 ```
___________________________


 ``` 
        public List<string> URL { get; set; } = new List<string>(); // Stores the Urls 
        public void OnGet()
        {
            string connectionString = "CONNECT_STR";


            BlobServiceClient blobServiceClient = new(connectionString);
            string containerName = "testblobfromapp0af289d0-1413-46f6-b59d-b7b6cbd434c8";

            BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName);

            var blobUrl = containerClient.Uri.AbsoluteUri;  // Gets the url for the container

            var blobs = containerClient.GetBlobs();         // Gets the Images
            foreach (var b in blobs)
            {
                 URL.Add($"{blobUrl}/{b.Name}"); // Adds all of them to the list
            }

        }
 ```
___________________________

 ``` 
 @page
@model PrivacyModel
@{
    ViewData["Title"] = "Images";
}
<h1>@ViewData["Title"]</h1>



@if (Model.URL != null)
{
    foreach (var item in Model.URL)
    {
        <img src="@item" alt="" style="max-height: 300px; max-width: 400px"> // displays the images on the page 
    }

}

 ```
________________________

If this would go on for about a year in that pace for around a year the cost would be around $20,000. ALOT more then I expected!

________________________

### What Azure Does
They make sure your data is encrypted when published to Azure, and placed on disk and decrypted when accessed using Azure Storage Service Encryption. You can also use be authenticated using public-key cryptography and you can use the Asure Key Vault for storing API keys, passwords, certificates, or cryptographic keys.

_________________________

<https://www.learnrazorpages.com/razor-pages/forms/file-upload>    
<https://www.bmc.com/blogs/saas-vs-paas-vs-iaas-whats-the-difference-and-how-to-choose/>  
<https://docs.microsoft.com/en-us/azure/key-vault/general/basic-concepts>