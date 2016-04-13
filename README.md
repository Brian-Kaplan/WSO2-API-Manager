# WSO2-API-Manager

Server Repo for Mobile Single Sign on Solution

# Documentation

Installation
  - on Mac or Linux cd into WSO2-API-Manager/wso2am-1.9.0/bin/ and run the server ./wso2server.sh
    - It may yell at you to set the JAVA_HOME variable, refer to this [https://docs.wso2.com/display/APPM100/Installing+on+Linux+or+Mac+OS+X] WSO2 Documentation
    
  - Once the server is up the command line will show you three different server addresses
  
      Mgt Console URL  : https://172.30.79.172:9443/carbon/
      
      API Publisher Default Context : http://172.30.79.172:9763/publisher
      
      API Store Default Context : http://172.30.79.172:9763/store
      
  - Log in = admin, password = admin
    - This is the only user currently created with all permissions
  
API Store
  - The API store currently has two instances of the same API at two slightly different endpoints
    http://172.30.79.172:8280/phoneverify/1.0.0/CheckPhoneNumber
    http://172.30.79.172:8280/phoneverify2/1.0.0/CheckPhoneNumber
  - They do the exact same thing but represent two different end points for the two Third Party Applications
    - HokieBooks connects to phoneverify
    - HokieDiner conncets to phoneverify2
    
API Publisher
  - Use this interface to publish a new API
    - Refer to this documentation to make the APIs that are currently in the manager
      https://docs.wso2.com/display/AM180/Quick+Start+Guide#QuickStartGuide-CreatinganAPI
      
OAuth Configurations
  - There are three Service Providers set up within this server
  - Each one contains its own OAuth Configuration with its own Client Key and Client Secret
  
  - Navigate to the list of service providers (Left Sidebar)
    - Click Edit on any of the providers
      - Click Inbound Authentication Configuration then OAuth/OpenID Connect Configuration
        - The Client ID Client Secret and Callback must match in the Third Party Apps OAuthConstants.java class
      
  
  
  
