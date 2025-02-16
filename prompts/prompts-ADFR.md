# Using Cursor with Claude 3.5 Sonnet

## First Prompt

You are a Github pipelines expert. We are in a project with a frontend and a backend, and I want to create a Github actions pipeline inside the file @pipeline-ADFR.yml that starts with the trigger "git push to an existing branch that has an open pull request". Our deploy target will be an existing AWS account.

This pipeline must have steps to:
1. Launch the backend tests.
2. Create a backend build.
3. Deploy the built backend in AWS EC2

Let's go step by step, and please ask me any questions before creating any code

### Answer

I'll help you create the GitHub Actions pipeline. Before proceeding, I need to clarify a few things based on the codebase you provided:


1. I can see your backend is a Node.js/TypeScript application using Jest for testing. From the package.json:

```6:13:backend/package.json
    "scripts": {
        "start": "node dist/index.js",
        "dev": "ts-node-dev --respawn --transpile-only src/index.ts",
        "build": "tsc",
        "test": "jest",
        "prisma:init": "npx prisma init",
        "prisma:generate": "npx prisma generate",
        "start:prod": "npm run build && npm start"
```

2. I notice you have AWS configuration information in your README files, but I need to confirm:

   - Do you already have an EC2 instance set up?
   - Do you have the AWS credentials (AWS_ACCESS_ID and AWS_ACCESS_KEY) configured in your GitHub repository secrets?
   - Is the EC2_INSTANCE (IP or DNS) also configured in your secrets?

3. For the deployment:

   - Are you planning to use PM2 for process management on EC2? (I see it mentioned in your setup instructions)
   - Do you want to set up Nginx as a reverse proxy? (Also mentioned in your setup)
   - Should we include environment variables deployment?
   
   Please confirm these details so I can provide you with the most appropriate pipeline configuration.

## Second prompt

2. 
  - Yes, I have an EC2 instance already setup
  - Yes
  - Yes

3. 
 - Yes
 - Yes
 - Yes
  
### Answer

Thank you for confirming those details. Based on your codebase and requirements, I'll help you create the GitHub Actions pipeline. Let's create the pipeline step by step.
First, I'll create the basic structure with the trigger you specified and then we'll add each step:

...

Before proceeding, I need to confirm a few additional details:

1. What AWS region should we use? I've set it to `eu-west-1` by default.
2. I notice you're using PM2. What's the name of your PM2 process? I've used "backend" as default.
3. I see you have a `.env` file. Should we add a step to copy environment variables during deployment?
4. What's the path of your application on the EC2 instance? I've used `/home/ec2-user/app/backend` as default.

Also, you'll need to add one more secret to your GitHub repository:
- `EC2_SSH_KEY`: The private SSH key to connect to your EC2 instance

Please confirm these details and I can adjust the pipeline accordingly.

## Third prompt 

1. We will use us-east-1
2. Let's use "backend"
3. Yes
4. Perfect

I've added this new secret

### Answer

Great! I'll update the pipeline with these changes. Here's the modified version:

...

The main changes are:
1. Changed AWS region to `us-east-1`
2. Added the `.env` file creation step before building

You'll need to add one more secret to your GitHub repository:
- `DATABASE_URL`: Your PostgreSQL connection string

The pipeline will now:
1. Run tests
2. Build the application
3. Connect to AWS
4. SSH into your EC2 instance
5. Pull the latest code
6. Install dependencies
7. Create the .env file with the database connection
8. Build the TypeScript code
9. Restart the PM2 process

Would you like me to explain any part of this in more detail?

## Fourth prompt

No, I'm going to test it

### Answer

Great! Go ahead and test the pipeline. If you encounter any issues, feel free to come back and I'll help you troubleshoot them. Good luck!


