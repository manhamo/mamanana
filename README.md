#https://github.com/stacksimplify/aws-eks-kubernetes-masterclass/tree/master/08-ELB-Application-LoadBalancers/08-01-ALB-Ingress-Install
#https://github.com/RobinNagpal/kubernetes-tutorials/tree/master/06_tools/007_alb_ingress/01_eks
#https://medium.com/aws-lambda-serverless-developer-guide-with-hands






## Install Apache

```
#!/bin/bash
yes | sudo apt update
yes | sudo apt install apache2
echo "<h1>Server Details</h1><p><strong>Hostname:</strong> $(hostname)</p><p><strong>IP Address:</strong> $(hostname -I | cut -d" " -f1)</p>" > /var/www/html/index.html
sudo systemctl restart apache2
```

## Command for creating `foo`,  `bar` pages

```
sudo mkdir /var/www/html/foo
sudo mkdir /var/www/html/bar

sudo touch /var/www/html/foo/index.html
sudo touch /var/www/html/bar/index.html

sudo vi /var/www/html/foo/index.html
<h1>Hello from /foo demo-nlb-ec2-1</h1><p>This is a sample hello message in /foo.html.
sudo vi /var/www/html/bar/index.html
<h1>Hello from /bar demo-nlb-ec2-1</h1><p>This is a sample hello message in /bar.html.</p>"
```


```
https://docs.aws.amazon.com/whitepapers/latest/moderating-image-content-in-slack/create-a-lambda-function-to-process-messages-where-image-references-were-found-via-sqs-queue.html
```
 cdc
```
https://medium.com/@aaloktrivedi/integrating-lambda-to-send-and-receive-messages-from-sqs-and-sns-part-2-6595133dd1a3
```


```
https://medium.com/@aaloktrivedi/integrating-lambda-to-send-and-receive-messages-from-sqs-and-sns-part-1-5b359f0f1678
```
loadbalancer
https://www.youtube.com/watch?v=fZuxp_pOzgI





```
import json
import os #access our environment variables
import boto3 #access AWS resources and methods

QEUE_RESIZE = os.environ['QEUE_URL']
QEUE_DL= os.environ['D_QEUE_URL']
def lambda_handler(event, context):
    #metod should be post
    data=json.loads(event['body'])
    
    sqs = boto3.resource('sqs')
    rqeue = sqs.Queue(QEUE_RESIZE)
    dqeue=sqs.Queue(QEUE_DL)
    url=data['url']
    action=data['action']
    try:
        if action== "resize":
            response = rqeue.send_message(MessageBody=url) #send message to queue      
            response = f"MessageID: '{response['MessageId']}' sent to queue."
            print(response)
            return{ 
                'statusCode': 200,
                'body': response
            }
            
        elif action == "download":
            response = dqeue.send_message(MessageBody=url) #send message to queue  
            response = f"MessageID: '{response['MessageId']}' sent to queue."
            print(response)
            return{ 
                'statusCode': 200,
                'body': response
            }
            
    except Exception as e:
        print(e)
        return "sth went wrong"

```
