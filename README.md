# AWS SQS event redrive Lambda

This repository contains one simple AWS Lambda function in Python to redrive AWS SQS events from source queue to destination queue. This script can be triggered periodically via cloudwatch as well if source queue contains too many events and you dont want to redrive them  altogether.
* batch_size can be passed as input which will move only X events in one execution cycle. Default batch size is 10 if no value is passed.
* This script ensures that if same event is getting moved into DLQ again and again, it wont get retried more than 3 times.

**Disclaimer**: This script adds one additional field *'RetryCounter'* to keep track of number of retries in SQS's MessageAttributes. In 99.9% of cases, ideally this shouldn't cause any break in consumer code, because this is just an additional field added in attributes (NOT in body), but please confirm the impact once before usage.
None of the existing data of SQS event is touched upon.

## Setup
1. Create/Open AWS account
2. Go to Lambda service
3. Click on create new Lambda
4. Give a suitable name to your lambda function.
5. Choose python 3.6 or above as 'Runtime'
6. Click on 'Create Function'
7. Copy and paste the code present in 'SQSEventsRedrive.py' file in this repository in the Lambda.
8. Click on 'Deploy' to deploy this code in lambda server.
9. Click on Test and provide the input to the lambda function as explained in below section and submit the request.
10. You should see the data getting moved from source to destination queue.
11. In case of any issues, you can check the logs in cloudwatch by clicking on Existing lambda main page -> Monitoring -> Cloudwatch Logs -> see recent file and recent logs there.

## Sample Input
```
{
  "source_queue_url" : "AWS source sqs queue URL",
  "destination_queue_url" : "AWS destination sqs queue URL",
  "batch_size": 10
}
```
### **Note**: 
* Only *'batch_size'* is optional here. queue URLs are **mandatory** fields. 
* Resource URLs are expected in the input, not the ARNs.

