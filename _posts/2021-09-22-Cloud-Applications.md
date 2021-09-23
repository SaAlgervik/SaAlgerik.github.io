---
layout: post
title: Applications In the Cloud
subtitle: Pay when it's used
cover-img: https://images.unsplash.com/uploads/141103282695035fa1380/95cdfeef?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1730&q=80
thumbnail-img: https://images.unsplash.com/photo-1586282391129-76a6df230234?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1470&q=80
share-img: /assets/img/path.jpg
tags: [Function-as-a-Service , Serverless]
---

#### Application?
The application has a simple Razor Page with one Get and on Post. On the ***index.html*** we added one button, and when it's clicked on the data from our Cosmos Db will be presented on the page. Below we have a textbox that take in a new user with a submit button and post this to the Cosmos Db. If you would like to test it, you can do that [here](https://webappazureclass20210922104053.azurewebsites.net/).


_______________________
### Code

```
nanamespace WebAppAzureClass.Pages
{
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;
        public List<User> Users { get; set; } = new List<User>();
        [BindProperty]
        public string NewUserName { get; set; }

        private readonly string ConnectionString = "AccountEndpoint=https://cosmosdbwithfuncton.documents.azure.com:443/;AccountKey=ffutxQHVEYypHZP9keNhQYlEM6a1eZAT0BzJ4fkGo4l9oyPcSEO9O1dG2kuTY6jaqC1G26DoZ7JfHXOLQzL8jA==;";

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

       

        public async Task<IActionResult> OnGetUsers()
        {
            CosmosClient cosmosClient = new CosmosClient(ConnectionString); // Sets the connection to Cosmos function
            Database database = cosmosClient.GetDatabase("UserDatabase");   // Sets database to get from
            Container container = database.GetContainer("UserContainer");  // Sets tha container to get from

            // Hämta och presetera data ifrån CosmosDb

            string sqlQueryText = "SELECT * FROM c";                       // Query to execute 

            QueryDefinition definition = new QueryDefinition(sqlQueryText); // Sets the query

            var iterator = container.GetItemQueryIterator<User>(definition); // Finds all the matching data in that container with the query
            
            while (iterator.HasMoreResults)                                   
            {
                foreach (var item in await iterator.ReadNextAsync())
                {
                    var user = new User
                    {
                        Id = item.Id,
                        Name = item.Name
                    };
                    Users.Add(user);                                      //  Adds the users found to the Users List
                }
            }
            return Page();                                                 // Returns the data to the view
        }

        public async Task<IActionResult> OnPost()
        {
            CosmosClient cosmosClient = new CosmosClient(ConnectionString);
            Database database = cosmosClient.GetDatabase("UserDatabase");
            Container container = database.GetContainer("UserContainer");
            var user = new User                                             //Creats a new user with new id 
            {
                Id = Guid.NewGuid().ToString(),
                Name = NewUserName
            };
            try
            {
               await container.CreateItemAsync<User>(user, new PartitionKey(user.Name));      //Post it to tha database
            }
            catch (Exception)
            {

                throw;
            }
            return Page();                                      
        }
    }
}
public class User                               //Class to storde the temp data from database
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName = "Name")]
    public string Name { get; set; }

}
```
_______________________
### How It Works
So we started with opening up a new web appliction in Visual Studio and then Published it to Azure as a App Service using [this](https://docs.microsoft.com/en-us/azure/app-service/quickstart-dotnetcore?tabs=netcore31&pivots=development-environment-vs) guide. After that we made the code you see above and tested it in the local host starting with the ***Get*** method. When we got the respons as we wanted we started with the ***Post***. When all of it worked as we wanted it to we made published it again.  

After this we started with making the pipeline work in GitHub with som help from [here](https://docs.microsoft.com/sv-se/azure/app-service/deploy-ci-cd-custom-container?tabs=acr&pivots=container-linux).


_______________________
### Cost

For just a few users the cost whould be around $78.11 and for alot of users around $2,604 so this is something to think about!

_______________________


<https://docs.microsoft.com/sv-se/azure/app-service/deploy-ci-cd-custom-container?tabs=acr&pivots=container-linux>    
<https://docs.microsoft.com/en-us/azure/devops/boards/github/connect-to-github?view=azure-devops>  