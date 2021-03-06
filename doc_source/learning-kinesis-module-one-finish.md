# Step 7: Finishing Up<a name="learning-kinesis-module-one-finish"></a>

Because you are paying to use the Kinesis stream, make sure you delete it and the corresponding DynamoDB table once you are done with it\. Nominal charges occur on an active stream even when you aren't sending and getting records\. This is because an active stream is using resources by continuously "listening" for incoming records and requests to get records\.

**To delete the stream and table**

1. Shut down any producers and consumers that you may still have running\.

1. Open the Kinesis console at [https://console\.aws\.amazon\.com/kinesis](https://console.aws.amazon.com/kinesis)\.

1. Choose the stream that you created for this application \(`StockTradeStream`\)\.

1. Choose **Delete Stream**\.

1. Open the DynamoDB console at [https://console\.aws\.amazon\.com/dynamodb/](https://console.aws.amazon.com/dynamodb/)\.

1. Delete the `StockTradesProcessor` table\.

## Summary<a name="learning-kinesis-module-one-summary"></a>

Processing a large amount of data in near real time doesn’t require writing any magical code or developing a huge infrastructure\. It is as simple as writing logic to process a small amount of data \(like writing `processRecord(Record)`\) but using Kinesis Streams to scale so that it works for a large amount of streaming data\. You don’t have to worry about how your processing would scale because Kinesis Streams handles it for you\. All you have to do is send your streaming records to Kinesis Streams and write the logic to process each new record received\. 

Here are some potential enhancements for this application\.

**Aggregate across all shards**  
Currently, you get stats resulting from aggregation of the data records received by a single worker from a single shard \(a shard cannot be processed by more than one worker in a single application at the same time\)\. Of course, when you scale and have more than one shard, you may want to aggregate across all shards\. This can be done by having a pipeline architecture where the output of each worker is fed into another stream with a single shard, which is processed by a worker that aggregates the outputs of the first stage\. Because the data from the first stage is limited \(one sample per minute per shard\), it would easily be handled by one shard\.

**Scale processing**  
When the stream scales up to have many shards \(because many producers are sending data\), the way to scale the processing is to add more workers\. You can run the workers in EC2 instances and leverage Auto Scaling groups\.

**Leverage connectors to S3/DynamoDB/Redshift/Storm**  
As a stream is continuously processed, its output can be sent to other destinations\. AWS provides [connectors](https://github.com/awslabs/amazon-kinesis-connectors) to integrate Kinesis Streams with other AWS services and third\-party tools\.

## Next Steps<a name="learning-kinesis-module-one-next-steps"></a>

+ For more information about using Kinesis Streams API operations, see [Developing Amazon Kinesis Streams Producers Using the Amazon Kinesis Streams API with the AWS SDK for Java](developing-producers-with-sdk.md), [Developing Amazon Kinesis Streams Consumers Using the Amazon Kinesis Streams API with the AWS SDK for Java](developing-consumers-with-sdk.md), and [Managing Kinesis Streams Using Java](working-with-streams.md)\.

+ For more information about Kinesis Client Library, see [Developing Amazon Kinesis Streams Consumers Using the Kinesis Client Library](developing-consumers-with-kcl.md)\. 

+ For more information about how to optimize your application, see [Advanced Topics](advanced-consumers.md)\. 