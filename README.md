## Udagram Image Filtering Microservice

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into three parts:
1. [The Simple Frontend](https://github.com/udacity/cloud-developer/tree/master/course-02/exercises/udacity-c2-frontend)
A basic Ionic client web application which consumes the RestAPI Backend.
2. [The RestAPI Backend](https://github.com/udacity/cloud-developer/tree/master/course-02/exercises/udacity-c2-restapi), a Node-Express server which can be deployed to a cloud service.
3. [The Image Filtering Microservice](https://github.com/udacity/cloud-developer/tree/master/course-02/project/image-filter-starter-code), the final project for the course. It is a Node-Express application which runs a simple script to process images.

### Instruction for Running a Local Instance of the Server

1. Clone the project and change directory to the new created folder.
2. Run npm `npm i` to initialize the project and install project dependencies.
3. Run the development server with `npm run dev`
4. Open http://localhost:8082 to test the service.
5. Try the image filter microservice using after adding the image_url parameter to the following endpoint http://localhost:8082/filteredimage?image_url=REMOTE_IMAGE_URL
6. You will see the resulted image displayed after applying the image filter to the original image.

### How to Deploy on ElasticBeanstalk

1. Install the EB CLI by reading the [AWS Doc Instructions](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html) for Install for your platform
2. Generating SSH Keypair to be used with EC2 instances. The keys you generate replace your password,
   but they should be treated as sensitive data that would grant anyone who holds them access to your
   running instance. AWS offers a great guide on how to 
   [create Key Pairs for your EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html). 
3. Navigate to the root project directory and run `eb init`. You will be asked different questions as the following:
   1. Select a default region: select your preferred default region or hit enter for selecting us-west-2 by default.
   2. Enter Application Name: Enter the name of the application or hit enter for selection the generated default name.
   3. It Appears that you are using Node.js. Is this correct?: enter `y` and hit enter for confirming.
   4. Do you wish to continue with CodeCommit?: enter `n` and hit enter.
   5. Do you want to set up SSH for your instance(s)?  
4. Add the following section to ./.elasticbeanstalk/config.yml file to configure Elastic Beanstalk to use generated build archive
   ```yaml 
   deploy:
      artifact: ./www/Archive.zip   
   ``` 
5. Run `npm run build` to transpile and package our code into a zip archive file for uploading and deployment.    
6. Run `eb create` and follow the on screen questions to set different parameters. You will be asked different questions as the following:
   1. Enter Environment Name (hit enter to set to the default).
   2. Enter DNS CNAME prefix (hit enter to set to the default).
   3. Select a load balancer type?: enter 2 (application load balancer) and hit enter.
   4. Would you like to enable Spot Fleet requests for this environment?: enter `n` and hit enter.
7. Elastic Beanstalk CLI will start to create the application and create the application environment in a few minutes.
8. After a few minutes, you will get the following messages "INFO  Successfully launched environment", which indicates that everything went as expected and the application and the environment was created successfully.
9. Open the AWS Elastic Beanstalk dashboard and select applications from the side menu then choose the application you created for this project. 
10. After choosing you application, you will see a public facing URL for your environment and for your deployed microservice under "URL" column in application details table (i.e. http://imagefilteringmicroservice-dev.us-east-1.elasticbeanstalk.com/).
11. To test the deployed Microservice, open a new tab in your web browser and enter the URL then hit enter.

### Troubleshooting

* Running `eb create` or `eb deploy` may result in the following error: "No start scripts located in package.json. Node.js may have issues starting. Add start scripts or place code in a file named server.js or app.js"
 This problem happens due to zipping the folder incorrectly. you can fix this error by following the next instructions:
  1. Delete your existing zipped folder.
  2. Go inside your code folder. You should see the package.json when you run ls command in your terminal.
  3. Run zip -r Archive.zip . to compress all the files and folders.
  4. Deploy the zipped folder on EB using Elastic Beanstalk console or using the command `eb deploy.