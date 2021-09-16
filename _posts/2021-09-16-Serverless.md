---
layout: post
title: Serverless
subtitle: Pay when it's used
cover-img: https://images.unsplash.com/photo-1617203441790-5723a811b7fe?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1100&q=80
thumbnail-img: https://images.unsplash.com/photo-1544980944-0bf2ec0063ef?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1100&q=80
share-img: /assets/img/path.jpg
tags: [Serverless]
---

#### What is serverless?
[Serverless](https://www.redhat.com/en/topics/cloud-native-apps/what-is-serverless) is a way to not have to worry about the hardware when it comes to scaling, updating and maintaining. There are still servers but they are abstracted away from app development. You **"simply"** package you code in containers for deployment instead. The difference between cloud services and serverless is that the apps is only launched when they are called on. This means that you will only pay for you app whan the code runs.

FaaS or  [Function-as-a-Service](https://www.redhat.com/en/topics/cloud-native-apps/what-is-faas) is a way of using business logic in Linux containers fully managed by a platform.  You can build a fully worik application with FaaS. What's good about it is that it is increases developers productivity and makes development time shorter. You don't have to make time for server management and it's scaling depends on the use of the service. You write a function in almost any languish.

Serverless and Faas is almoste the same thing but with time, serverless has grown to be more about larger sets of architectural patterns. Serverless can be used for microservies and is not just databases and messaging systems, as it used to be. 

_______________________

#### Calulator
We started with the templat that is built in to Visual Studio for Azure Functions HTTP and tryied it out. After we got how the code worked we just changed the "req.Query" to a int and added one more. We then added a "string output" that made the calculation and returnd the answer.
After that we puplished it to Azure Function and tried it with the URL given on Azure. The problem we ran in to was thad you had to have a "&" after the given URL to make your query from it and not a question mark as in the localhost.

After we got that part figured out the we tested it in teh browser aswell and it works perfectly, if you like to try it [here is the link](https://azuretesthttp.azurewebsites.net/api/Function1?code=kt/rMICG3aKh9q9b/HM9/jzLFRnwMjtZGrbxvQVQi9YvpQTUCKPizw==&first=1&second=3)

Change the "first = * " and "second = * " to the number you want.

_______________________
### Function

```
namespace FunctionApp1
{
    public static class Function1
    {
        [FunctionName("Function1")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            int first = int.Parse(req.Query["First"]);    # First query input
            int second = int.Parse(req.Query["Second"]);    # Second query input

            string output = $"{first + second}";    # Calculates the two numbers

            string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
            dynamic data = JsonConvert.DeserializeObject(requestBody);
            output = output ?? data?.name;

            string responseMessage = string.IsNullOrEmpty(output)     # Checks that it's not null or empty
                ? "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response."
                : $"{first } + {second} = {output}";    # The string to send back

            return new OkObjectResult(responseMessage);     # Returns the data
        }
    }
}

```
_______________________
### Risks    
The risk with injections is that you can, if you don't secure you code, write in executable code in the querystring. So even if you don't have a database connected to it's a risk you should think about.
To fix this for us we should make sure that you can't put anything els then numbers in the querystrin. We have not done this though...

_______________________


<https://raw.githubusercontent.com/OWASP/Serverless-Top-10-Project/master/OWASP-Top-10-Serverless-Interpretation-en.pdf>    
<https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview>  