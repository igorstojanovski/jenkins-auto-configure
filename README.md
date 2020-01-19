This is **not a production ready** image. It is intended for demonstration purposes only.

# Description
This project contains a Dockerfile that helps you build an Jenkins image who's configuration is completely in code.
The main configuration is placed in **configuration/jenkins.yml**. It sets up the main configuration.
The intention is to demonstrate that you can have Jenkins containers that you can start, stop, rebuild and start again whenever you can without the need to add any additional manual configuration after the restart.

This image is based on the official Jenkins LTS Docker image.

## What does it add to the main image?
* Turns off the first start installation wizard.
* Lets you install addition plugins that are listed in **configuration/plugins.txt**.
* One of those plugins is Jenkins Configuration as Code. That plugin will apply the configuration in **configuration/jenkins.yml** to your instance. You don't have to do anything manually.
* As part of the auto-configuration a seed job will be created. The seed job is configured with a [sample Sping Boot project](https://github.com/igorstojanovski/jenkins-pipeline-as-code) as the source
and a one minute trigger.
* Sets up basic authentication. The username and password you can find in **configuration/jenkins.yml**. This image is intended for demonstration purposes only
so the password is in plain text.  

## How is it intended to work?
By building the image and running the container you will get a Jenkins instance with all the necessary configuration, all the plugins installed and a seed job
that is configured to scan a [sample Sping Boot project](https://github.com/igorstojanovski/jenkins-pipeline-as-code) each minute. That project contains a Jenkinsfile
tha will be used to configure a whole pipeline which you can monitor with the installed Blue Ocean plugin.  

# Running the image
Clone the project and inside the project folder and build the image:
  
```docker build -t jenkins-auto .```

Run the image you just created:

```docker run -p 8080:8080 -p 5000:5000 --name jenkins-auto jenkins-auto```

# Tips
## Scripts that you can run in the Jenkins script console
* List all the installed plugins
```
Jenkins.instance.pluginManager.plugins.each{
  plugin -> 
    println ("${plugin.getShortName()}:${plugin.getVersion()}")
}
```
* Run a Job DSL script
```
Running groovy DSL from the Jenkins script console:

import javaposse.jobdsl.dsl.*
import  javaposse.jobdsl.plugin.*

JenkinsJobManagement jm = new JenkinsJobManagement(System.out, [:], new File('.'));
DslScriptLoader dslScriptLoader = new DslScriptLoader(jm)
dslScriptLoader.runScript("folder('project-a')")
```
 
 ## TO DO
 This image does not solve a couple of problems:
 * Monitoring is not set up
 * Logs are not externalized
     