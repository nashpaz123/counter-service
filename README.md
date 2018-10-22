# counter-service
This is a flask delivered python counter-service, dockerized web app. 
The app is available via port 80 on the Ubuntu 16 slave: http://ec2-34-253-223-252.eu-west-1.compute.amazonaws.com/

Usage:
1. On a Jenkins master, you need a Jenkins slave running docker 17 or greater, labeled "aws_ubuntu16".
2. Run the Jenkins file from a Multi-branch pipeline, to enable branch selection //Nash: I didn't add a branch param, as the Multi-branch pipeline deals with that for us.
3. Usage:
Build with parameter - Choose whether to build the docker image before deployment (Default is yes)

# The pipeline
1. ( optional, per parameter Build_Image value ) Builds the docker image, tags it
2. 
   - Stop existing redis and counter instances
   - Starts a new redis instance
3. Deploys the image to a new instance
4. Tests the app on localhost

Please see the implementation at:
http://ec2-18-202-248-214.eu-west-1.compute.amazonaws.com:443/job/counter-service/job/MainDev/
Or 
http://ec2-18-202-248-214.eu-west-1.compute.amazonaws.com:443/job/counter-service/job/master/




