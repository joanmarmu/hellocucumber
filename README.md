In this quick tutorial you will learn how to:

- Install Cucumber
- Write your first Scenario using the Gherkin syntax
- Write your first step definition in JavaScript
- Run Cucumber
- Learn the basic workflow of Behaviour-Driven Development (BDD)
- We’ll use Cucumber to develop a small library that can figure out whether it’s Friday yet.

Before we begin, you will need the following:

+ Node.js
+ A text editor


STEP 1 - Create an empty Cucumber project 🔗︎
We’ll start by creating a new directory and an empty Node.js project.

mkdir hellocucumber
cd hellocucumber
npm init --yes
Add Cucumber as a development dependency:

npm install cucumber --save-dev
Open package.json in a text editor and change the test section so it looks like this:

{
  "name": "hellocucumber",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "cucumber-js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "cucumber": "^5.0.3"
  }
}
Prepare the file structure:

mkdir features
mkdir features/step_definitions
Create a file called cucumber.js at the root of your project and add the following content:

module.exports = {
  default: `--format-options '{"snippetInterface": "synchronous"}'`
}
Also, create a file called features/step_definitions/stepdefs.js with the following content:

const assert = require('assert');
const { Given, When, Then } = require('cucumber');
You now have a simple project with Cucumber installed.

Verify Cucumber installation 🔗︎
To make sure everything works together correctly, let’s run Cucumber.

# Run via NPM
npm test

# Run standalone
./node_modules/.bin/cucumber-js
You should see something like the following:

0 Scenarios
0 steps
0m00.000s
Cucumber’s output is telling us that it didn’t find anything to run.
