## Use the power of script steps in your Systems Manager Automation runbooks
The CFN template (EncryptedVolsToSlack.yaml) will stand up infrastructure in AWS Config, IAM, and Systems Manager to send information about unencrypted volumes to a Slack Channel. It showcases the Automation runbook action aws:executeScript in order to perform an API calls to AWS and Slack. 

![Architecture Diagram](https://github.com/aws-samples/aws-systems-manager-executescript-slack/blob/main/ssm-executescriptdiagram.png)
1.	AWS Config runs the encrypted-volumes rule to find EBS volumes where the encryption flag is not set.
2.	For each unencrypted EBS volume, AWS Config invokes an automatic remediation that executes a Systems Manager Automation runbook.
3.	The runbook uses aws:executeScript to retrieve information about the EBS volume.
4.	The runbook uses aws:executeScript to:
  a.	Retrieve an AWS Secrets Manager secret that contains the Slack URL and channel information.
  b.	Post the information to the Slack channel.

## Prerequisites
For the integration with Slack, follow these instructions to add a Slack Incoming Webhook. Choose a Slack channel to send information to and receive a URL with a prefix of https://hooks.slack.com/workflows/.... for posting the information. Save the information in AWS Secrets Manager secret with the following format:
{
  "URL": "TheSlackUrl",
  "channel": "TheSlackChannel"
}


AWS Config must be enabled in your AWS account for your region, and must monitor at least EC2:Instance and EC2:Volume types. See [Getting Started with AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/getting-started.html) for more info.

## Tips
### Getting output from aws:executeScript step
Within the python code:

`return {'message': theMsg }`

In the <strong>output</strong> section:

```yaml
outputs:
       - Name: ebsInfoMsg        
         Selector: $.Payload.message         
         Type: String
```

### Passing data into python code within aws:executeScript step
In the <strong>InputPayload<strong> section, declare the variables you need and use double {{ }} to reference parameters:
  
```yaml
InputPayload:
             ebsVolumeId: '{{ResourceId}}' 
```
             
Within the python code, you can reference the variables declared in InputPayload using the <strong>events</strong>:

`events['ebsVolumeId']`


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

