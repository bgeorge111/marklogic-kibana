# marklogic-kibana
## Objective
 * Quickly set up a MarkLogic server and the ELK (Elastic - Logstash - Kibana) components to demonstrate how MarkLogic logs can be analysed in Kibana. The built-in configuration reads the log files 8000_ErrorLog.txt, 8011_ErrorLog.txt and 8011_AccessLog.txt. 
     
# Setting up the environment 

## Step 0 :: Pre-requisites 
* docker with docker-compose
* Understanding of the structure of docker-compose (required if changes to defaults are required) 
* Understanding of the filebeat, logstash, elasticsearch and kibana configurations (required if changes to defaults are required) 
* Internet connectivity

## Step 1 :: Create a network 
* Create a network using the below docker command. Keep the network name as demo-nw. If you want to change, make changes to env1.env file in the root. <br>
 ``` docker network create demo-nw ```
## Step 2 :: Review the configurations (Optional)
* The default configurations should be fine in most cases. But, in case of possible port conflicts etc, take a look \*-compose.yml files. 

## Step 3 :: Build the environment 
* Run the below docker command from the ``` root ``` folder <br>
``` docker-compose --env-file env1.env –f marklogic-compose.yml -f elastic-compose.yml up –d –build ``` <br>
If all good, four containers will be running 
* marklogic1001 _(MarkLogic server with myadmin/myadmin as admin credentials and BASIC as the authentication mechanism) _
* logstash
* elasticsearch
* kibana 

Additionally, filebeat agent will be installed and started on the marklogic container. The different components and purpose are illustrated below
![image](https://user-images.githubusercontent.com/68338060/208221697-7e1aa197-e644-4c89-aa8f-0580e0857029.png)

## Usage
* Generate some log entries to the 8000 App server Ex. xdmp.log("Line1\nLine2", "errors") generates one multi-line entry to 8000_ErrorLog.txt. 
* Since filebeat is already started in MarkLogic, the 8000_ErrorLog.txt is being pushed to logstash for aggregation. 
* Access the Kibana UI at http://<name>:5601 
* Go to Home -> Stack Management -> Kibana -> Index Patterns -> Create Index Pattern 
	ml_8000_error as a index source will be available to create the pattern. 
* Go to Home -> Analytics -> Discover to see the logs in Kibana. The multi-line MarkLogic logs will be already aggregated as a single entry in Kibana
* Create Kibana dashboards as you wish. 

## Configurations
The configuration files of ELK stack is simple to understand. The default configurations should be ok for a simple demonstration of how MarkLogic multi-line logs can be brought into Kibana. Below are the configuration files involved 
	
Filebeat : 
* ```marklogic/filebeat-error/filebeat.yml``` : example filebeat configuration for MarkLogic error log files
* ```marklogic/filebeat-access/filebeat.yml``` : example filebeat configuration for MarkLogic access log files
Logstash:
* ```logstash/pipeline/marklogic-errorLog-pipeline.conf``` : example logstash pipeline configuration for MarkLogic error log files
* ```logstash/pipeline/marklogic-accessLog-pipeline.conf``` : example logstash pipeline configuration for MarkLogic access log files
* ```logstash/config/pipelines.yml``` : example logstash configuration to support multiple pipelines

	

	
