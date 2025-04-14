 Problem Statement:
 You work for XYZ Corporation. Your corporation wants the files to be stored on a private repository on the AWS Cloud. Once done, you are required to automate a few tasks for ease of the development team.
 
 Tasks To Be Performed:
 1. Create a website in any language of your choice and push the code into GitHub.
 2. Migrate your GitHub repository into the AWS CodeCommit repository.
 3. Create two CodeDeploy deployments (for the QA stage and the Production stage) with an EC2 deployment group into which you can push the code from the CodeCommit repository.
 4. Using AWS CodePipeline, create a software development life cycle:
 a. The source is the CodeCommit repository
 b. The code will be pushed into the deployment created in CodeDeploy
 c. There should be two stages in deployment, the QA stage and the Production stage
 d. Only when the QA stage is successful, the Production stage should be executed
 5. Create a third stage where the same website is pushed into an Elastic Beanstalk environment.
 
 Note: The application can be of any language, or it can even be a sample application. The purpose here is to create a CI/CD pipeline, not to create a great application
