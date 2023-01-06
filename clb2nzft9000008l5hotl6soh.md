# A path to hosting personal projects for free on the cloud

Having created a series of personal projects, I was excited to start building a number of projects of mine, but after being orphaned by the [discontinuation of Heroku's free plans](https://blog.heroku.com/next-chapter), the first challenge was to host them. To start my search, I had some prerequisites:

*   **Reliability first**. I want to sleep well at night and not worry if my applications are down because of an unstable cloud provider
    
*   **Use managed services instead of DIY solutions**, so I could focus on my code. Instead of being dragged down by a lot of manual labor and spending a huge amount of time doing things like applying updates to my server's OS or doing manual backups of my database, I would rather think about the problem that I am trying to solve and the code that is unique to my application
    
*   **Have the lowest cost possible**, as I live in Brazil and paying in dollars really hurts my wallet
    
*   **Use industry standards**. I do not want to invest my time learning about platforms that are only used by hobbyists or a small niche, I would rather learn a single platform that could be used to power either a simple personal project or a large, enterprise-grade solution
    
*   The solution should be **able to do back-end tasks**: it should be capable of doing authentication, user sign-in/sign-up, a persistent database for storing user data, as well as static page hosting and be able to create APIs with ease.
    

## A little surprise

So I looked over the cloud provider options and all of them offered similar services. AWS has the largest market share, so it was the natural first choice. At the very basic, I needed a place to run my server code and a database. On the server hosting, I could get going for a fully managed solution like [AWS Lightsail](https://aws.amazon.com/lightsail/) on containers for $7/month.

On the database side, things did not look great. Even if I start with a single project with very little database use, the cheapest [AWS RDS](https://aws.amazon.com/rds/) instance would charge about $30/month. Another option would be the managed database provided by AWS Lightsail, resulting in a $15/month cost.

Still, I would have to spend at least $22 per month. Some of the readers from developed countries may find it strange that I making a big fuss over 20 dollars, but here in Brazil, it is expensive, especially when those are only experimental projects with no plans in the short term for revenue. On top of that, it is not even an ideal solution, as most of the time the database server would be sitting doing nothing, meaning wasted dollars. And it is not very scalable either, as spinning another project using the same kind of resources quickly adds more dollars to the monthly cost.

## Going serverless

Going back and forth in [AWS Pricing Calculator](https://calculator.aws), I discovered that serverless services are mostly free when there is only light usage (thanks to their [Free Tier](https://aws.amazon.com/free/)) and, even with some respectable loads, they are very inexpensive, meaning a monthly cost that wouldn't top the $5 or $10 mark. On top of that, those are managed services, meaning I would only focus on code that matters to me, no time wasted managing or monitoring the infrastructure.

Before continuing, I would like to clarify that serverless is not a type of service that does not use servers, but rather a **totally** managed service, meaning that everything is taken care of by AWS: thinking about how to balance the traffic load between servers or to create a private network between a server and an API is not needed because AWS takes care of the servers, the overall infrastructure, the way it scales up or down, the way it handles traffic and other low-level configurations. In general, It is possible to create resources that would receive zero requests and would stay available for free or they can receive millions or billions of requests and AWS takes care of everything.

## What if I use AWS services to replace my current web framework?

At a first glance, AWS services seemed to be a bunch of services that would need to be manually created, configured and connected together, going back and forth on their webpage. But going a bit deeper, it became clear that is not the only way that it is meant to be used.

[AWS services can be used as an entire framework](https://spiegelmock.com/2021/05/29/frameworkless-web-applications-aws-cdk/), like Django or Flask, and [it may even be the next generation of web frameworks](https://mitelman.engineering/blog/amazon-api-gateway-with-lambda-is-a-next-generation-of-web-frameworks/), so I would write application code in my favorite programming language and also infrastructure code, describing that I need authentication, API, storage, for example, and a very brief configuration for each of these services. Then I would be able to test the code, store it in version control and deploy all the infrastructure, back-end and front-end code using a single command. If I need to add a new service, it would be just like adding a new feature in my application code: change the code, test it, send it to version control and deploy it.

All of this is possible thanks to AWS CDK. It can generate AWS resources with very little code because it uses sensible defaults, so it is not necessary the describe all the details, like in [AWS Cloudformation](https://aws.amazon.com/cloudformation/). CDK generates an AWS Cloudformation template, which is **MUCH** more verbose and has all the necessary details that are not specified in the original CDK code, then the template is used to deploy the services at AWS. Here is an AWS CDK code that is describing an API that uses a Lambda function to process the requests:

```python
from constructs import Construct
from aws_cdk import (
    Stack,
    aws_lambda as _lambda,
    aws_apigateway as apigw,
)


class CdkWorkshopStack(Stack):

    def __init__(self, scope: Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)
        
        # Creates a AWS Lambda resource and points where the Python code is located
        my_lambda = _lambda.Function(
            self, 'HelloHandler',
            runtime=_lambda.Runtime.PYTHON_3_7,
            code=_lambda.Code.from_asset('lambda'),
            handler='hello.handler',
        )
        
        # Defines the my-lambda instance as endpoint for the API
        apigw.LambdaRestApi(
            self, 'Endpoint',
            handler=my_lambda,
        )
```

That's it! Deploying this code will result in a simple API being deployed, ready to receive requests. Doing the same thing on the AWS Console page would mean a bunch of clicking, scrolling, configuring all the permissions and doing all the work again for each different AWS region. In this example, all necessary authorizations for the services to interact with each other are automatically created and defaults are applied automatically if the developer does not mention it in the CDK code.

For those that are already familiar with a framework called Flask, a serverless AWS equivalent is available as [AWS Chalice](https://aws.github.io/chalice/) and is **very** similar to Flask in terms of syntax. By using it, it is possible to integrate authentication or a database as one would have done if using Flask, writing a line or two of code. The snippet below creates with a few lines an API with GET and POST routes that create Users and Profiles. Both routes use a database, provided by DynamoDB, and the POST routes use AWS Cognito to provide authentication and sign-in/sign-out flows:

```python
import os
import boto3
from chalice import Chalice, CognitoUserPoolAuthorizer


app = Chalice(app_name='cdkdemo')
dynamodb = boto3.resource('dynamodb')
dynamodb_table = dynamodb.Table(os.environ.get('APP_TABLE_NAME'))

# Creates an Cognito Authorizer for the API
authorizer = CognitoUserPoolAuthorizer(
    'cdk-test-pool',  # Name of authorizer
    [os.environ.get('USER_POOL_ID')] # Gets User Pool ARN
)


@app.route('/users', methods=['POST'], authorizer=authorizer)
def create_user():
    """ Creates or updates a user in DynamoDB Table """
    # Gets json payload in request body
    request = app.current_request.json_body

    # Creates an item
    item = {
        'PK': f'User#{request["username"]}',
        'SK': f'Profile#{request["username"]}',
    }

    # Item is created or updated in database
    dynamodb_table.put_item(Item=item)

    # Return empty dictionary
    return {}


@app.route('/users/{username}', methods=['GET'])
def get_user(username):
    """ Gets a user from DynamoDB Table """
    # Creates a key to search for
    key = {
        'PK': f'User#{username}',
        'SK': 'Profile#{username}',
    }

    # Gets key from db
    item = dynamodb_table.get_item(Key=key)['Item']

    return item
```

AWS Chalice automatically creates everything that is needed for the application to run in the AWS cloud and can neatly integrate into AWS CDK. Sure, it is possible to achieve the same result using only Lambda functions, but it can make the writing of application code much more convenient.

## Code Organization

Some of you could be wondering how to organize the infrastructure and the application code. Both should be part of the same repository and be separated into different folders, like in the example below:

```plaintext
├── infrastructure
│   ├── app.py #AWS infrastructure definitions are here
│   ├── requirements.txt #Requirements used by infrastructure definition
├── runtime
│   ├── app.py #Chalice code is here
│   ├── requirements.txt #Requirements used by Chalice app
└── requirements.txt #Contains pointers to both requirements
```

Each infrastructure and runtime should have its own tests, but they should share the same virtual environment. To install all dependencies you only use the root folder requirements.txt file, it only contains pointers to the other 2 requirements files.

## Summing up

Learning and using AWS services has been a long journey for me. I had a fair share of frustration and despair when I came across unexpected errors and confusing documentation from AWS and it adds a ton of complexities over something like Heroku. Thinking back on what I have experienced so far, I could sum up a few downsides when working with AWS:

### Cons

*   Learning the inner workings of many AWS services and their relationships have a steep learning curve.
    
*   It has a very different logic compared to traditional, non serverless frameworks that take some time to get used to it
    
*   Vendor lock-in. Using multiple AWS services and investing time to learn them means that it will be harder to move to another cloud provider, also moving data and users that are already stored in AWS to other equivalent services could be very painful. Think about moving from Apple to Android or vice versa
    
*   The costs of serverless can be high if the services are not configured properly and end up consuming more server time than they should or if they receive very high loads
    
*   The AWS documentation, besides being bland, sometimes is outdated or is not very polished. Information about AWS Cognito, for example, is spread between different AWS websites: in documentation, CDK documentation and articles.
    

But the advantages outweigh the disadvantages:

### Pros

*   Serverless is a modern way of creating infrastructure with flexibility and low costs
    
*   After overcoming the learning curve, it becomes easy to build or replicate complex infrastructure that has proven itself in production in very little time
    
*   AWS services have seamless integration, especially if using AWS CDK
    
*   Zero cost or little cost, but is able to respond to spikes in demand and manage huge loads
    
*   AWS is used by many companies, has a large community and knowledge of serverless is in high demand
    
*   AWS has plenty of free or paid learning resources like videos, tutorials and articles, created by AWS or other third parties
    
*   Instead of paying a full salary for a DevOps or database specialist, you paid a little monthly fee for AWS that can be reduced or canceled at any time, no hard feelings
    

That's all for today! If you have some comments or questions, feel free to ask in the comments below.