# Deploy Reactjs & Laravel with subdirectory using NGINX

#Overview
This what you will need to know if you are building a react-app on top of (ASP.NET Web API) any project on one single domain. So the API will sit at the root and the UI in a sub-folder in order to access Active Directory credentials for an Intranet application.

#React App
First step is to update the **package.json** file from the root of your application. Add an entry for **"homepage"** in the same section with ***“name”** towards the top of the JSON file. The URL should be the domain plus the sub-folder.
```
"homepage":"/Project-1"
```

