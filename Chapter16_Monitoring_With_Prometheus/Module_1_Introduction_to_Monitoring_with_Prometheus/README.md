# Chapter16
In this module we will introduce monitoring with Prometheus

# Name: Introduction to Monitoring with Prometheus


# Description: 
Introduction to Prometheus
  

# Usage
Introduction to Prometheus Monitoring

    What is Prometheus?

        Monitoring Tool
         
            Designed for Highly dynamic container environments

                Container and Microservices Infrastructure

                K8s     Docker Swarm (Container cluster)

            Traditional Bear Metal

            Why is Prometheus so important?


    Where and why is Prometheus is used?

        DevOps is becoming more complex

            100s of processes

            which are interconnected

                Need to know what is happending on hardware and on applications

                Errors?

                Response Latency?

                Overloaded?

                Do we have enough resources?

                Identify bugs

            

            Some Use Cases

                Server ran out of memory

                Kicked off two K8s pods responsible for managing db sync, caused to fail

                DB was used by authentication service, which stopped working 

                Application was dependent on Auth service, could not authenticate user in the UI

                From users perspective experiencing authentication issues

                *How do you know what went wrong?

                *Error: "Authentication Failed"

                Work backwards from there to find root cause.

                    Is application backend running?

                    Any exceptions?

                    Is Auth-service running?

                    Why did Auth-service crash?

                    All the way to the initial container failureQ

                What would make this more efficient?

                    Constantly monitor all the services

                    Alert when service crashes

                    










    Prometheus Architecture

    Example Configuration

    Key Characteristics




  

