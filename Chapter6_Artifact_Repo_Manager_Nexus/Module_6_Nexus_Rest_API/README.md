# Chapter6
This module we will explore the Nexus API

# Name: Nexus API

# Description: 

Nexus API - Introduction

Need the endpoint in order to query Nexus Repository for different information

    which components are available?

    what are the versions?

    which repositories

This information is needed in your CI/CD Pipeline

    Build > Test >  Release >   Deploy >    Operate

When you are pushing multiple artifacts per day

Flexible

How to access the REST endpoint?

    Use a tool like curl or wget to execute http request

    Provide user and credential of a Nexus user

    Use the Nexus user with the required permissions

       curl -u user:pwd -X GET 'http://157.230.238.59:8081/service/rest/v1/repositories'  




The Nexus user that was created only has limited access to the repos based on what was configured

    curl -u michael:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/repositories'

The admin user has full access to see all repos configured in Nexus

    curl -u admin:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/repositories'


All repositories displayed in json format



    curl -u admin:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/components?repository=maven-snapshots'

        Contains all the artifacts and metadata for this specific component, based on the information that was pushed.  Each has it's own list  of assets, which are the files.

    "items" : [ {
    "id" : "bWF2ZW4tc25hcHNob3RzOjg4NDkxY2QxZDE4NWRkMTM2MzI2OGYyMjBlNDVkN2Rl",
    "repository" : "maven-snapshots",
    "format" : "maven2",
    "group" : "com.example",
    "name" : "my-app",
    "version" : "1.0-20240619.023722-1",
    "assets" : [ {
      "downloadUrl" : "http://157.230.238.59:8081/repository/maven-snapshots/com/example/my-app/1.0-SNAPSHOT/my-app-1.0-20240619.023722-1.jar",
      "path" : "com/example/my-app/1.0-SNAPSHOT/my-app-1.0-20240619.023722-1.jar",
      "id" : "bWF2ZW4tc25hcHNob3RzOjJlNDdkZGEwZjFiNTU1ZTA3MTU5ZGM5ZjlkZDNmZWY0",
      "repository" : "maven-snapshots",
      "format" : "maven2",
      "checksum" : {
        ...

    "id" : "bWF2ZW4tc25hcHNob3RzOmMwYWMyYWI2YzVlOTNhNGE3NmY3OTkxMDI5ZTA5NGM4",
    "repository" : "maven-snapshots",
    "format" : "maven2",
    "group" : "com.example",
    "name" : "java-maven-app",
    "version" : "1.1.0-20240619.034810-1",
    "assets" : [ {
      "downloadUrl" : "http://157.230.238.59:8081/repository/maven-snapshots/com/example/java-maven-app/1.1.0-SNAPSHOT/java-maven-app-1.1.0-20240619.034810-1.jar",
      "path" : "com/example/java-maven-app/1.1.0-SNAPSHOT/java-maven-app-1.1.0-20240619.034810-1.jar",
      "id" : "bWF2ZW4tc25hcHNob3RzOjQwMjkyYWNkZWJjMDFiODM4YTZmMWRlYmE0MzYyMWM1",
      "repository" : "maven-snapshots",
      "format" : "maven2",
      "checksum" : {
        "sha1" : "94a35857a87c5f42a570e4e70629f443c0e5a16f",
        "sha512" : "f7a68a2c902ac2bf3c69d0de85ed0a129cd43d2f4eaeb960ac34428091c1940463848b3041a31390e5e10523d2b54a594c7edba82c4d547f2dc2512dd3a5b643",
        "sha256" : "b461f67611668b4e3d511eecd5895ca12f259c9cbcef0ca2eadafd9f29accc61",
        "md5" : "c4e5cf110f0511ed5e5c6e818d2ae429"
        ...



    Can perform query on a component based on id

    curl -u admin:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/components/bWF2ZW4tc25hcHNob3RzOjg4NDkxY2QxZDE4NWRkMTM2MzI2OGYyMjBlNDVkN2Rl'




# Usage

michaelbradley@Michaels-iMac-2 new-project %  curl -u michael:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/repositories'
[ {
  "name" : "maven-snapshots",
  "format" : "maven2",
  "type" : "hosted",
  "url" : "http://157.230.238.59:8081/repository/maven-snapshots",
  "attributes" : { }
} ]%   


michaelbradley@Michaels-iMac-2 new-project %  curl -u michael:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/repositories'
[ {
  "name" : "maven-snapshots",
  "format" : "maven2",
  "type" : "hosted",
  "url" : "http://157.230.238.59:8081/repository/maven-snapshots",
  "attributes" : { }
} ]%                                                                                                                                                                                                          michaelbradley@Michaels-iMac-2 new-project %  curl -u admin:Greater16 -X GET 'http://157.230.238.59:8081/service/rest/v1/repositories'
[ {
  "name" : "nuget-hosted",
  "format" : "nuget",
  "type" : "hosted",
  "url" : "http://157.230.238.59:8081/repository/nuget-hosted",
  "attributes" : { }
}, {
  "name" : "maven-snapshots",
  "format" : "maven2",
  "type" : "hosted",
  "url" : "http://157.230.238.59:8081/repository/maven-snapshots",
  "attributes" : { }
}, {
  "name" : "nuget.org-proxy",
  "format" : "nuget",
  "type" : "proxy",
  "url" : "http://157.230.238.59:8081/repository/nuget.org-proxy",
  "attributes" : {
    "proxy" : {
      "remoteUrl" : "https://api.nuget.org/v3/index.json"
    }
  }
}, {
  "name" : "maven-central",
  "format" : "maven2",
  "type" : "proxy",
  "url" : "http://157.230.238.59:8081/repository/maven-central",
  "attributes" : {
    "proxy" : {
      "remoteUrl" : "https://repo1.maven.org/maven2/"
    }
  }
}, {
  "name" : "nuget-group",
  "format" : "nuget",
  "type" : "group",
  "url" : "http://157.230.238.59:8081/repository/nuget-group",
  "attributes" : { }
}, {
  "name" : "maven-public",
  "format" : "maven2",
  "type" : "group",
  "url" : "http://157.230.238.59:8081/repository/maven-public",
  "attributes" : { }
}, {
  "name" : "maven-releases",
  "format" : "maven2",
  "type" : "hosted",
  "url" : "http://157.230.238.59:8081/repository/maven-releases",
  "attributes" : { }
} ]%                                       
    