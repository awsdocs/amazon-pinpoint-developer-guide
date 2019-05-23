# Next Steps<a name="tutorials-importing-data-next-steps"></a>

By completing this tutorial, you've done the following:
+ Created an Amazon S3 bucket that will contain the files that you import into Amazon Pinpoint\.
+ Created several IAM roles and policies that follow the principle of least privilege\.
+ Created Lambda functions that split your input file into smaller parts, process those files, and then use them to create an import job in Amazon Pinpoint\.
+ Set up your Amazon S3 bucket to initiate Lambda functions when it detects files in certain folders\.

This section discusses a few ways that you can customize this solution to fit your unique use case\.

## Perform Cleanup Tasks<a name="tutorials-importing-data-next-steps-cleanup"></a>

After you complete this tutorial, you can delete the input\_archive, to\_process, and processed folders entirely\. These folders are recreated automatically the next time the functions in this tutorial are executed\.

This solution doesn't automatically delete the files in the processed folder\. If an import job fails, you can try to import the files again by creating a new import job by using the console or the API\.

Over time, this folder might accumulate a large number of files that you don't need anymore\. You can create a script that periodically removes old content from this folder\. If you do, you should include logic that checks to see if there are any ongoing import jobs before deleting any files\.

## Modify the Internationalization Function to Suit Your Customer Database<a name="tutorials-importing-data-next-steps-internationalization"></a>

One of the Lambda functions that you create in Step 5 performs some simple tests to normalize the country and phone number data that it processes\. This function includes tests for the most populous countries in the world, as well as some countries that Amazon Pinpoint customers commonly send messages to\. You can expand these tests to include additional countries and regions\.

## Modify the Input Processing Function to Suit Your External Systems<a name="tutorials-importing-data-next-steps-map-columns"></a>

The other Lambda function that you create in Step 5 creates the column names that Amazon Pinpoint expects to see, and maps them to the columns that are in your input spreadsheet\. You can change this mapping by making some small changes to the function\. The comments in the included code tell you specifically what you need to change\.

## Automatically Synchronize Data with External Systems<a name="tutorials-importing-data-next-steps-sync"></a>

If you regularly use data from external systems to send campaigns in Amazon Pinpoint, you might be able to set up the external system to regularly export data to the input folder in your Amazon S3 bucket\. If you do, make sure that each file that you export has a unique name\. If you don't supply unique names, the Lambda function that creates import jobs will fail, because it has some simple logic to prevent the creation of duplicate import jobs\.