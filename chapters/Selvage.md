# Serverless computing

James Selvage

It was in 2016 at a summit in London that I first heard the “serverless” phrase and I was both confused and intrigued. It was a few talks in when one of the presenters said, “of course there are servers …” Phew that sounds more understandable.

So what is serverless computing? Of course there are servers, but you do stuff without managing them. The major cloud service providers offer serverless computing:
* Amazon Web Services Lambda functions
* Google Cloud functions
* Microsoft Azure functions
* IBM OpenWhisk

The one that I am most familiar with is Amazon Web Services (AWS) Lambda. Let’s look at an AWS provided example:
> “New function using blueprint s3-get-object-python”

Here’s the Python 2.7 Lambda function:

    from __future__ import print_function
    import json
    import urllib
    import boto3
    print('Loading function')
    s3 = boto3.client('s3')
    def lambda_handler(event, context):
        print("Received event: " + json.dumps(event, indent=2))
        # Get the object from the event and show its content type
        bucket = event['Records'][0]['s3']['bucket']['name']
        key = urllib.unquote_plus(event['Records'][0]['s3']['object']['key'].encode('utf8'))
        try:
           response = s3.get_object(Bucket=bucket, Key=key)
           print("CONTENT TYPE: " + response['ContentType'])
           return response['ContentType']
        except Exception as e:
           print(e)
           print('Error getting object {} from bucket {}. Make sure they exist and your bucket is in the same region as this         function.'.format(key, bucket))
           raise e
               
Once this Lambda function is created in your AWS Account you can link it to a trigger, which for this function is an S3 bucket (storage in the cloud) that is going to be populated with different files. Each time a file is put in the bucket:
* event triggers (input) 
* lambda function starts (server starts) 
* content type is printed (output)
* function ends (server stops)

No files added = no servers running, 1 file added = 1 server starts & stops, n files added = n servers start & stop. That’s serverless…

It is a different way of developing applications and it requires a different way of thinking. By joining together multiple functions you can create an Application Programming Interface (API). At Osokey we’ve recently created an API for extracting meta data, viewing and processing SEG Y files. Each function is limited to a specific task, e.g. read the EBCDIC header from a SEG Y file, and each function is configured with different memory to meet its specific requirements. Multiple APIs can be joined together, e.g. a horizon viewing API can be joined with the SEG Y viewing API. These APIs are callable by third parties, run on-demand and are scaleable.

A complete platform for geosciences could be built by creating further APIs. How about an API for?
* Well log QC
* Rock physics
* Full waveform inversion
* …

At the same summit I also heard Conway’s Law, “Organizations which design systems … are constrained to produce designs which are copies of the communication structures of these organizations”. Melvin Conway was not trying to be humorous, but have a think about the organisation that you work in and the systems that have been designed. 

In terms of communication, outside of work we have almost all adopted cloud-based solutions. Slack, Facebook, Google Services, WhatsApp, …  have all become hugely popular because they provide instant access from wherever we are. We don't need to have physical photo albums, video libraries or music collections any more. This transition has happened without us really noticing and many of the tools integrate seamlessly into our lives. Why not the same for the oil & gas industry and the way we work?

Cloud services provide the flexible IT platform for scaling business up and down, but to be fully agile we also need to "virtualise" the workforce. Current working practises are a historical accident - why do we all travel to an office and work largely independently for eight hours? Would it not be better to vary our working hours according to the work that is actually required? This is particularly the case in E&P when we are so dependent on external circumstances - licensing rounds, the price of oil, availability of farm-ins, etc. 

Communication structures outside and inside organisations are changing and this is leading to different system designs. Most companies in the oil & gas industry have their own cloud strategy, but the whole industry will miss an opportunity if these system are not integrated.              