# Instructions for installing the cubicSvr image

1. Send your AWS account id to Cobalt to grant you permission to the appropriate repo

2. Authenticate with the image repository in aws:

   `aws ecr get-login --no-include-email --registry-ids 494415350827`

   This will return an auth token for docker that is valid for 12 hours. (There are other ways of authenticating, but this seems to be the easiest.  See [the ECR documentation](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html#registry_auth) for alternatives or if you run into trouble.  You might also need to change your region to us-east-1 before you call get-login.)

3. Copy, paste and execute the whole line that's returned from the get-login command. You can even do this on a different machine if you want to install on a box that doesn't have the AWS CLI.  It will look something like this:

   `docker login -u AWS -p someridiculouslylongtoken https://4944-1535-0827.dkr.ecr.us-east-1.amazonaws.com`
 
4. Pull the image that includes the model you want (either 8kHz or 16kHz)

   `docker pull 494415350827.dkr.ecr.us-east-1.amazonaws.com/cubicsvr-demo-en_us-8`

   OR

   `docker pull 494415350827.dkr.ecr.us-east-1.amazonaws.com/cubicsvr-demo-en_us-16`

5. Start the cubic service listening:

   `docker run -p 2727:2727 -p 8080:8080  -name cobalt 494415350827.dkr.ecr.us-east-1.amazonaws.com/cubicsvr-demo-en_us-16`

   That will start listening for grpc commands on port 2727 and http requests on 8080, and will stream the debug log to stdout.  (BTW, you can replace -name cobalt with whatever name you want.  That just provides a way to refer back to the currently running container.)

6. Verify the service is running by calling 
   `curl http://localhost:8080/api/version`

   There are [4 API endpoints](API.md).  One of them, StreamingRecognize, requires a stream input, so it must either use websockets or grpc, and therefore can’t be called using curl.

7.  If you want to explore the package to see the model files etc,
   `docker exec -it cobalt bash`
   opens the bash terminal on the previously run image.  Model files are located in the /model directory.
