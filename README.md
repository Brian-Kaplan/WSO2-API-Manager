# WSO2-API-Manager and Android SDK

Server Repo for Mobile Single Sign on Solution

[WSO2 API Manager Version 1.9.0](http://wso2.com/more-downloads/api-manager/)

SDK
  - The android SDK for use with 3rd party mobile apps is included in this repo
  - build the src into a .jar from the root directory. It will be inside the target directory when finished
  
    ```
    $ mvn clean install
    ```

# Documentation

Installation
  - You will have to download a new server instance and follow set up instructions. We've found the server to not be very "portable". I will try to walk you through everything we did to set up a server. 
  - [Download WSO2 API Manager 1.9.0](http://wso2.com/more-downloads/api-manager/) Binary 1.9.0
  - Follow the installation instructions [here](https://docs.wso2.com/display/AM170/Installing+the+Product)
    
  - Once the server is up the command line will show you three different server addresses
  
      The management console

      Mgt Console URL  : https://localhost:9443/carbon/
      
      The publisher to create new APIs

      API Publisher Default Context : http://localhost:9763/publisher
      
      The store to view and subscribe applications to APIs

      API Store Default Context : http://localhost:9763/store
      
  - Log in = admin, password = admin
    - This is the only user currently created with all permissions
  
API Store
  - After following the set up instructions for the HokieBooks and HokieDiner apps you should have the two APIs below
  - These two instances of the same API at two slightly different endpoints
    http://localhost:8280/phoneverify/1.0.0/CheckPhoneNumber
    http://localhost:8280/phoneverify2/1.0.0/CheckPhoneNumber
  - They do the exact same thing but represent two different end points for the two Third Party Applications
    - HokieBooks would connect to phoneverify
    - HokieDiner would conncet to phoneverify2
    
API Publisher
  - Use this interface to publish a new API
    - Refer to [this](https://docs.wso2.com/display/AM180/Quick+Start+Guide#QuickStartGuide-CreatinganAPI) documentation to make the APIs that are currently in the manager
      
      
OAuth Configurations
  - There are three Service Providers set up within this server
  - The only one you need to set up manually is for the IDP App, instructions at that repo
  - The other two will be set up automatically after configuring the APIs and subscribing to them
  - Each one contains its own OAuth Configuration with its own Client Key and Client Secret
  
  - Navigate to the list of service providers (Left Sidebar)
    - Click Edit on any of the providers
      - Click Inbound Authentication Configuration then OAuth/OpenID Connect Configuration
        - The Client ID Client Secret and Callback must match in the Third Party Apps OAuthConstants.java class
  
Token Management
  - Token management is handled via the [Token API](https://docs.wso2.com/display/AM190/Token+API)
  - Here are some curl commands for various things
  
    - Generate access token with password grant_type
    
      ```
      $ curl -k -d "grant_type=password&username=<USERNAME>&password=<PASSWORD>" -H "Authorization: Basic <base64 encoded CLIENTID:CLIENTSECRET>, Content-Type: application/x-www-form-urlencoded" https://localhost:8243/token
      ```
      
    - Generate access token with code grant_type (this is what the Third Party Applications will use)
    
      ```
      $ curl -k -d "grant_type=code&code=<CODE>" -H "Authorization: Basic <base64 encoded CLIENTID:CLIENTSECRET>, Content-Type: application/x-www-form-urlencoded" https://localhost:8243/token
      ```
      
    - Revoke an access token
    
      ```
      $ curl -k -d "token=<TOKEN>" -H "Authorization: Basic <base64 encoded CLIENTID:CLIENTSECRET>" https://localhost:8243/revoke
      ```
      
    - Refresh an access token with refresh_token grant_type (each refresh token can only be used once)
      
      ```
      curl -k -d "grant_type=refresh_token&refresh_token=<REFRESH_TOKEN>" -H "Authorization: Basic <base64 encoded CLIENTID:CLIENTSECRET>" -H "Content-Type: application/x-www-form-urlencoded" https://localhost:8243/token
      ```
    - Access an API endpoint that is setup with this server instance
    
      ```
      curl -H "Authorization :Bearer <ACCESS_TOKEN>" -v 'http://localhost:8280/phoneverify/1.0.0/CheckPhoneNumber?PhoneNumber=<PHONE_NUMBER>&LicenseKey=0'
      
      curl -H "Authorization :Bearer <ACCESS_TOKEN>" -v 'http://localhost:8280/phoneverify2/1.0.0/CheckPhoneNumber?PhoneNumber=<PHONE_NUMBER>&LicenseKey=0'
      ```
  
  
  
