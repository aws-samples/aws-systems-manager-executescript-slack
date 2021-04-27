## Use the power of script steps in your Systems Manager Automation runbooks
The CFN template (EncryptedVolsToSlack.yaml) will stand up infrastructure in AWS Config, IAM, and Systems Manager to send information about unencrypted volumes to a Slack Channel. It showcases the Automation runbook action aws:executeScript in order to perform an API callss to AWS and Slack. 

1.	AWS Config runs the encrypted-volumes rule to find EBS volumes where the encryption flag is not set.
2.	For each unencrypted EBS volume, AWS Config invokes an automatic remediation that executes a Systems Manager Automation runbook.
3.	The runbook uses aws:executeScript to retrieve information about the EBS volume.
4.	The runbook uses aws:executeScript to:
  a.	Retrieve an AWS Secrets Manager secret that contains the Slack URL and channel information.
  b.	Post the information to the Slack channel.


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

