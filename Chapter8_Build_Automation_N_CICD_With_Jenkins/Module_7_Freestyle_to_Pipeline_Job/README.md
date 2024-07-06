# Chapter8
This module we will look at Freestyle to Pipeline Job.

# Name: Freestyle to Pipeline Job

# Description: 

In the previous steps we performed:


    Build Java App ----> Run Tests ----> Build Image ---->Push to private Repo

    Common practice with Freestyle Job is to execute 1 job/step

    Freestyle Job - Integration

    Freestyle Job - Build the Application

    Freestyle Job - Testing the Application

    Freestyle Job - Deploying, etc

If you needed another step using Freestye you could use Post-build Actions to trigger step 2 if build was stable

    This was done before the pipeline job was created, but with limitations

    It was built when UI based steps were used, before scripting became popular

    This meant you had to use plugins

    But, you were limited to what the plugin allowed you to do - limited to input fields of plugin

    Not suitable for complex workflows

With the emergence of CI/CD and Infrastructure as Code this became obsolete



A DevOps Engineer should now be working with Pipeline Jobs


    Suitable for CI/CD

    Scripting - Pipeline as code


Will begin to understand the fundamental advantages and disadvantages 

    When using plugins

    Executing tools

    



# Usage


    