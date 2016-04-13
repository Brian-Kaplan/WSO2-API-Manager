# WSO2-API-Manager and Android SDK

Server Repo for Mobile Single Sign on Solution

[WSO2 API Manager Version 1.9.0](http://wso2.com/more-downloads/api-manager/)

SDK
  - The android SDK for use with 3rd party mobile apps is included in this repo
  - $ mvn clean install will build the src into a .jar inside the target directory

# Documentation

Installation
  - on Mac or Linux cd into WSO2-API-Manager/wso2am-1.9.0/bin/ and run the server ./wso2server.sh
    - It may yell at you to set the JAVA_HOME variable, refer to [this](https://docs.wso2.com/display/APPM100/Installing+on+Linux+or+Mac+OS+X)  WSO2 Documentation
    
  - Once the server is up the command line will show you three different server addresses
  
      Mgt Console URL  : https://localhost:9443/carbon/
      
      API Publisher Default Context : http://localhost:9763/publisher
      
      API Store Default Context : http://localhost:9763/store
      
  - Log in = admin, password = admin
    - This is the only user currently created with all permissions
  
API Store
  - The API store currently has two instances of the same API at two slightly different endpoints
    http://localhost:8280/phoneverify/1.0.0/CheckPhoneNumber
    http://localhost:8280/phoneverify2/1.0.0/CheckPhoneNumber
  - They do the exact same thing but represent two different end points for the two Third Party Applications
    - HokieBooks connects to phoneverify
    - HokieDiner conncets to phoneverify2
    
API Publisher
  - Use this interface to publish a new API
    - Refer to [this](https://docs.wso2.com/display/AM180/Quick+Start+Guide#QuickStartGuide-CreatinganAPI) documentation to make the APIs that are currently in the manager
      
      
OAuth Configurations
  - There are three Service Providers set up within this server
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
  
  
  
