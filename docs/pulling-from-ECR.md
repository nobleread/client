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
   
