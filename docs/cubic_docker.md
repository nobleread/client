# Instructions for installing the cubicSvr image

You should have received a link to an image file in AWS S3.  This link will expire in two weeks, so be sure to download the file to your own server.

1. Download with the AWS CLI if you have it, or with curl:
   ```
   URL="the url sent by Cobalt"
   IMAGE_NAME="name you want to give the file (should end with the same extension, usually bz2)"
   curl $URL -L -o $IMAGE_NAME
   ```
   
2. Load the docker image: `docker load < $IMAGE_NAME`

   This will output the name of the image (e.g. 494415350827.dkr.ecr.us-east-1.amazonaws.com/cubicsvr-demo-en_us-16).

3. Start the cubic service listening:

   `docker run -p 2727:2727 -p 8080:8080 --name cobalt 494415350827.dkr.ecr.us-east-1.amazonaws.com/cubicsvr-demo-en_us-16`

   That will start listening for grpc commands on port 2727 and http requests on 8080, and will stream the debug log to stdout.  (BTW, you can replace `--name cobalt` with whatever name you want.  That just provides a way to refer back to the currently running container.)

4. Verify the service is running by calling 
   `curl http://localhost:8080/api/version`

   There are [4 API endpoints](README.md).  One of them, StreamingRecognize, requires a stream input, so it must either use websockets or grpc, and therefore can’t be called using curl.

5.  If you want to explore the package to see the model files etc,
   `docker exec -it cobalt bash`
   opens the bash terminal on the previously run image.  Model files are located in the /model directory.
   
## Contents of the docker image
### Base docker image ### 
debian-stretch-slim

### Additional dependency ###
(installed with yum install on centos or apt-get on ubuntu): libgfortran3

### Cobalt-specific files ###
- **cubicsvr** - binary for performing Automatic Speech Recognition
- **model.config** - top-level config
- **am/nnet3_online/final.mdl** - this is the acoustic model
- **am/nnet3_online/conf/online_cmvn.conf** - feature extraction parameters for the features fed into the GMM model used for i-vector statistics accumulation.
- **am/nnet3_online/conf/splice.conf** - GMM feature context when accumulating statistics for i-vector accumulation. 
- **am/nnet3_online/ivector_extractor/*** - Kaldi configuration files related to ivectors  
- **graph/HCLG.fst** - the decoding graph: the combination of the AMs transition graph, the lexicon, and the language model
- **graph/words.txt** - an integer to word mapping, needed because the output of the HCLG graph contains only integer symbol IDs
- **graph/phones/word_boundary.int** - this is needed only when confusion network output is requested, it tells the decoder which phones are at word boundaries
- **formatter/*** - if present, provides a model for text formatting of the output (e.g. capitalization). This formatter is only used if specified in model.config.
