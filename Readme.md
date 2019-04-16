#Encryption and decryption for the SNS - SQS scenario
## KMS
AWS KMS is a managed service that enables you to easily encrypt your data. AWS KMS provides a highly available key storage, management, and auditing solution for you to encrypt data within your own applications and control the encryption of stored data across AWS services.

## SQS
Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components.

Amazon SQS supports both standard and FIFO queues but Amazon SNS isn't currently compatible with FIFO queues.

####Encryption in transit :

For SNS- SQS encryption, Amazon SNS by default supports in-transit encryption based on Amazon Trust Services(ATS)

####Encryption at rest :

Server-side encryption (SSE) lets you transmit sensitive data in encrypted queues and protects the contents of messages in Amazon SQS queues using keys managed in AWS Key Management Service (AWS KMS).

Amazon SQS generates data keys based on the AWS-managed customer master key (CMK) for Amazon SQS or a custom CMK to provide envelope encryption and decryption of messages for a configurable time period (from 1 minute to 24 hours).

SSE encrypts messages as soon as Amazon SQS receives them. The messages are stored in encrypted form and Amazon SQS decrypts messages only when they are sent to an authorized consumer.

The producer must have the kms:GenerateDataKey and kms:Decrypt permissions for the customer master key (CMK).The consumer must have the kms:Decrypt permission for any customer master key (CMK) that is used to encrypt the messages in the specified queue. 

####Subscribing SQS Queues to SNS topics
We can subscribe one or more Amazon SQS queues to an Amazon SNS topic from a list of topics available for the selected queue. 

Amazon SQS manages the subscription and any necessary permissions. When you publish a message to a topic, Amazon SNS sends the message to every subscribed queue.

####Enable Compatibility between Amazon SNS with Encrypted Queues

To allow Amazon SNS topic subscriptions to work with encrypted queues, we need to allow SNS to have the kms:GenerateDataKey* and kms:Decrypt permissions i.e,need to add the following statement to the policy of the CMK.

{
   "Version": "2012-10-17",
      "Statement": [{
         "Effect": "Allow",
         "Principal": {
            "Service": "service.amazonaws.com"
         },
         "Action": [
            "kms:GenerateDataKey*",
            "kms:Decrypt"
         ],
         "Resource": "*"
       }]
}



