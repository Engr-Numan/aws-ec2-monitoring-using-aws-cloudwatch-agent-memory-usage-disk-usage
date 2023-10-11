# aws-ec2-monitoring-using-aws-cloudwatch-agent-memory-usage-disk-usage
aws-ec2-monitoring using aws cloudwatch agent memory usage, disk usage

How to setup Ec2 Instance  Monitoring by custom metric Disk_Usage and Memory Usage Using AWS cloudwatch agent:::::::::::::::::::

Prerequisites:
Launch aws instance ubuntu latest 

Once instance is in running state,  Go to IAM —-> Roles , Create Role—> AWS Service—->EC2—---->next—--> give name as per your own choice e.g ec2-monitoring role—-->  select role—-> awscloudwatchfull access ,   awscloudwatchagentserverpolicy, awscloudwatchagentadminpolicy

Now go to ec2-instance   select ec2-instace which is going to be monitored —go to actions —> security —-> modify iam role—--> then select here your already created role e.g ec2-monitoring role.---> update iam role



Now login to your ec2 instance 

For ubuntu Instance: Download Aws CloudWatchAgent .deb file using below command and install


# wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

# dpkg -i -E ./amazon-cloudwatch-agent.deb

# apt-get install collectd



By executing below command it will open the prompt and ask the question you will have to answer accordingly. 
 # /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard


Choose these settings below:


=============================================================
= Welcome to the AWS CloudWatch Agent Configuration Manager =
=============================================================
On which OS are you planning to use the agent?
1. linux
2. windows
default choice: [1]:
1
Trying to fetch the default region based on ec2 metadata...
Are you using EC2 or On-Premises hosts?
1. EC2
2. On-Premises
default choice: [1]:
1
Which user are you planning to run the agent?
1. root
2. cwagent
3. others
default choice: [1]:
1
Do you want to turn on StatsD daemon?
1. yes
2. no
default choice: [1]:
1
Which port do you want StatsD daemon to listen to?
default choice: [8125]
8125
What is the collect interval for StatsD daemon?
1. 10s
2. 30s
3. 60s
default choice: [1]:
3

What is the aggregation interval for metrics collected by StatsD daemon?
1. Do not aggregate
2. 10s
3. 30s
4. 60s
default choice: [4]:
4

Do you want to monitor metrics from CollectD?
1. yes
2. no
default choice: [1]:
2

Do you want to monitor any host metrics? e.g. CPU, memory, etc.
1. yes
2. no
default choice: [1]:
1
Do you want to monitor cpu metrics per core? Additional CloudWatch charges may apply.
1. yes
2. no
default choice: [1]:
1

Do you want to add ec2 dimensions (ImageId, InstanceId, InstanceType, AutoScalingGroupName) into all of your metrics if the info is available?
1. yes
2. no
default choice: [1]:
1

Would you like to collect your metrics at high resolution (sub-minute resolution)? This enables sub-minute resolution for all metrics, but you can customize for specific metrics in the output json file.
1. 1s
2. 10s
3. 30s
4. 60s
default choice: [4]:
4

Which default metrics config do you want?
1. Basic
2. Standard
3. Advanced
4. None
default choice: [1]:
slect advanced or Basic or Standard   i slected Advanced
Current config as follows:
{
    "agent": {
        "metrics_collection_interval": 60,
        "run_as_user": "root"
    },
    "metrics": {
        "metrics_collected": {
            "disk": {
                "measurement": [
                    "used_percent"
                ],
                "metrics_collection_interval": 60,
                "resources": [
                    "*"
                ]
            },
            "mem": {
                "measurement": [
                    "mem_used_percent"
                ],
                "metrics_collection_interval": 60
            },
            "statsd": {
                "metrics_aggregation_interval": 60,
                "metrics_collection_interval": 60,
                "service_address": ":8125"
            }
        }
    }
}

Are you satisfied with the above config? Note: it can be manually customized after the wizard completes to add additional items.
1. yes
2. no
default choice: [1]:

Do you have any existing CloudWatch Log Agent (http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html) configuration file to import for migration?
1. yes
2. no
default choice: [2]:
2
Do you want to monitor any log files?
1. yesf
2. no
default choice: [1]:
2







Now check status of the agent by using below command, it will show Stopped status.

# /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status

Now start the cloudwatch agent by using below  single command .

# sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json

Remember this command first copy to the notepad and paste in single line, then paste into the terminal otherwise it will through the error.



Now again check the status of it will show the running 

# /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status








Now go to the cloudwatch —-> all alarm, —--> create alarm —-> select metric,   —--> there should be cwagent shown. 

