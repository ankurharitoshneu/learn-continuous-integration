# Continuous Integration Tutorial

This repository contains sample Github Actions scripts to help understand how automatic continuous integration is desgined and implemented.

Read the complete reference to [Github Actions Workflow Syntax](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions) before proceeding. Refer to the slides on Canvas for a theoritical understanding of continuous integration.

## Repository structure

The repository has a Node server (`server.ts`) written using the Express framework. The server provides REST APIs to query books, authors, and details about specific books and authors. The services use a MongoDB database to store related data. The schema for the relevant MongoDB collections are defined in `models/`. The Express services communicate with the underlying MongoDB collections using the ORM/ODM Mongoose layer. Additonally, the `tests/` directory has a few Jest tests to verify the behavior of some of the services. 

## Setup Actions

One can setup any GitHub repository with GitHub Actions. This means based on events that happen on the repiository, a bunch of actions need to be taken. E.g., run tests everytime a branch is merged to main. Setting up actions is the first step to setup a continuous integration workflow. There are two ways to set this up -- a GitHub hosted runner or a self hosted runner. A GitHub hosted runner executes all the actions on the Github cloud. This as you can guess is not free. Hence, we will use a similar idea but will not cost us money. Therefore, we will setup a self hosted runner. A self hosted runner will execute actions on a system that configure as the runner. In this case, the runner system will be our own system. If you are working in a team, you have a pick a system that act as the runner. This has some limitations, specifically it tie your action scripts to underlying platform on which the runner is hosted. This is why in industry, teams prefer a cloud hosted runner. However, the approach of a self hosted runner will suffice for the purposes of this course, especially since it will allow us to learn the same concept but in a cheaper way. Follow the steps below to setup a self-hosted runner:

- In your forked repo naviagte to the **Settings** tab.
- In **Settings**, navigate to **Actions/Runner** on the left pane.
- It will say you have no runners configured.
- Click on **New self-hosted runner**. 
- Follow the instructions to setup and start the runner.
- When you configure the runner and start it, it will be listed the Runners page saying __Idle__ with a green color indicating is up and running.
- When the YAML workflow described below runs, it will run on this self-hosted runner, which happens to be your machine.

By following the above steps, you will have configured your machine to be self-hosted actions runner. A similar concept applies for runners configured to run on cloud servers.

## YAML Workflow

Imagine a team of developers working on this project. Anytime they make a change, they need to ensure that the existing functionality did not break. Hence, they design an automatic workflow that does the following:

Everytime code is pushed to the main branch, all jest tests run  and a report of the run is published for all team members to view. If the tests fail then the repository is in a bad state and needs to be fixed immediately before proceeding.

The team also maintains a deploy branch. This branch needs to be kept in a valid state at all times because, code from this branch is used in production. Therefore, the above workflow also gets triggered when a pull request is made to the deploy branch.

All this represented in the `.github/workflows/main.yml` file.

## For you to do

First setup your machine or your teammate's machine to be the self hosted runner using the instructions in __Setup__.

- Create a deploy branch
- Submit a pull request to the deploy branch from main
- Observe the actions branch in your repository.


Answer the following questions:

1. What does the __runs-on__ string  

Ans: 

The `runs-on` string specifies the type of machine that the job will run on. In this case, `self-hosted` means that the job will run on a self-hosted runner, which is a machine that you manage and configure yourself, rather than using GitHub's hosted runners.

2. In `main.yml`, on which branch do the jest tests run when a push to main branch is made?

Ans:

The Jest tests run on the main branch when a push is made to the main branch. This is specified in the on section of the main.yml file, which triggers the workflow on pushes to the main branch.

3. In `main.yml`, on which branch do the jest tests run when a pull request is submitted to the deploy branch?

Ans:

The Jest tests run on the deploy branch when a pull request is submitted to the deploy branch. This is specified in the on section of the main.yml file, which triggers the workflow on pull requests to the deploy branch.

Next, create a new workflow yml file that captures the following continuous integration requirement:

- When new changes are pushed to the deploy branch, the sample data should be setup using the scripts in `remove_db.ts` and `insert_sample_db.ts`.