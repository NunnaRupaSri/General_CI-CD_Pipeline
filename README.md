# General_CI-CD_Pipeline
    General CI/CD Pipeline index.html files runs on a EC2 Instance
    
    Its a general CI/CD pipeline to put an index.html file on EC2 instance using the Codebuild, codedeploy and codepipeline
    Source- Source code from Github repository

Code build:
    Its a managed build service in the cloud provider by the AWS, It designed to compile the source code, run unit tests and produce the deployable artifacts. Elimating the      need for users to provision, manage and scale their own build servers.
    * The build process is defined in buildspec.yml, which specifies the commands to run during the build phase (install, pre build, post build)
    
    Requriments for index.html:
    1. Basic Index.html file
    2. buildspec.yml file
    3. Role (Add the permission of S3 to put and get the object)
       
    AWS Console --> Codedeploy --> Create build project ---> Click on Create the build Project
                                    |__ Project Name
                                    |__ Source --> GitHub --> Manage the Account Credentials (3 possibilities)
                                                                  |__ Connections
                                                                  |__ Oauth
                                                                  |__ Token
                                    |__ Primary Source WebHook event (check the box --> Rebuild every time a code change is pushed to this repository)
                                    |__ Environment dont change any thing
                                    |__ Buildspec ---> select the option buildspec.yml 
                                    |__ Artificats dont change any thing
                                    |__ Logs (check the box --> Cloud watch logs)
    Pre configure the Notification in "Setting" in the codebuild, In "Build History" we can see the build history --> build run, successful/failed, artificates information
    
    ** Here html application is deploying on EC2 Instance (i.e In the codebuild we are not using any run time for html page, its different for other application)
    Java:
    --> java: corretto17 (runtime) in buildspec.yml we need to mention it in phases
    --> In phases pre post, post build we can define the echo --> description of need dependiencies
    --> In build phase we need to install the dependiencies (java dependiency is mvn)
    Python:
    --> python: 3.12 (runtime) in buildspec.yml we need to mention it in phases
    --> In phases pre post, post build we can define the echo --> description of need dependiencies
    --> In build phase we need to install the dependiencies (java dependiency is requriments.txt)
    
    So it buildspec.yml is varies on the application runtime. (java, python, Node.js ect...)
    
CodeDeploy:
    It is demployment service that automates the deployment to various compute services like AWS EC2 instance, AWS lambda serverless functions, Amazon ECS. It is a managed 
    services by the AWS provider. 
            Create Application ---> Create Deployment Grop
             |_ Application Name       |_ DG name
             |_ Compute Platform       |_ service role
                                       |_ codedeploy service role
                                       |_ Deployment type
                                       |_ Environment Configuration
                                       |_ Agent configuration with AWS Systems Manager 
                                       |_ Deployment settings
                                       |_Load balancer
            
    Deployment configurations: Based on configuration the application deployment can be compute on ec2, onpremises, lambda, lineary, canary 
    
    CodeDeploy reads the appspec.yml file from the source to manage the application deployment, file must be named exactly appspec.yml and placed in the root directory of         your application's source code.
    Appspec.yml file:
    |_ Version: The version of the AppSpec file format. The only valid value is 0.0.
    |_ os: The operating system of the target instances, either linux or windows
    |_ files: A mapping of source files in your revision bundle to their destination directories on the target instance.
    |_ hooks: A mapping of deployment lifecycle events to scripts that CodeDeploy should run at each stage

CodePipeline: 
    Is a fully managed, continuous delivery service that automates the release process for software and infrastructure updates
    CodePipeline uses a pipeline structure composed of stages and actions to define and automate your software release process.
    Stages:
    |_ Source
    |_ Build 
    |_ Test
    |_ Deploy







                                
