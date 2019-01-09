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


# STEP 1 - Create an empty Cucumber project
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


#STEP 2.1 Write a Scenario
When we do Behaviour-Driven Development with Cucumber we use concrete examples to specify what we want the software to do. Scenarios are written before production code. They start their life as an executable specification. As the production code emerges, scenarios take on a role as living documentation and automated tests.

Example Mapping

Try running an Example Mapping workshop in your team to design examples together.

In Cucumber, an example is called a scenario. Scenarios are defined in .feature files, which are stored in the features directory (or a subdirectory).

One concrete example would be that Sunday isn’t Friday.

Create an empty file called features/is_it_friday_yet.feature with the following content:

Feature: Is it Friday yet?
  Everybody wants to know when it's Friday

  Scenario: Sunday isn't Friday
    Given today is Sunday
    When I ask whether it's Friday yet
    Then I should be told "Nope"
The first line of this file starts with the keyword Feature: followed by a name. It’s a good idea to use a name similar to the file name.

The second line is a brief description of the feature. Cucumber does not execute this line, it’s just documentation.

The fourth line, Scenario: Sunday is not Friday is a scenario, which is a concrete example illustrating how the software should behave.

The last three lines starting with Given, When and Then are the steps of our scenario. This is what Cucumber will execute.

#STEP 2.2 See scenario reported as undefined
Now that we have a scenario, we can ask Cucumber to execute it.

npm test
Cucumber is telling us we have one undefined scenario and three undefined steps. It’s also suggesting some snippets of code that we can use to define these steps:

UUU

Warnings:

1) Scenario: Sunday is not Friday # features/is_it_friday_yet.feature:4
   ? Given today is Sunday
       Undefined. Implement with the following snippet:

         Given('today is Sunday', function () {
           // Write code here that turns the phrase above into concrete actions
           return 'pending';
         });

   ? When I ask whether it's Friday yet
       Undefined. Implement with the following snippet:

         When('I ask whether it\'s Friday yet', function () {
           // Write code here that turns the phrase above into concrete actions
           return 'pending';
         });

   ? Then I should be told "Nope"
       Undefined. Implement with the following snippet:

         Then('I should be told {string}', function (string) {
           // Write code here that turns the phrase above into concrete actions
           return 'pending';
         });


1 Scenario (1 undefined)
3 steps (3 undefined)
0m00.000s

Copy each of the three snippets for the undefined steps and paste them into features/step_definitions/stepdefs.js .

# STEP 2.3 See scenario reported as pending
Run Cucumber again. This time the output is a little different:

P--

Warnings:

1) Scenario: Sunday is not Friday # features/is_it_friday_yet.feature:4
   ? Given today is Sunday # features/step_definitions/stepdefs.js:3
       Pending
   - When I ask whether it's Friday yet # features/step_definitions/stepdefs.js:8
   - Then I should be told "Nope" # features/step_definitions/stepdefs.js:13

1 Scenario (1 pending)
3 steps (1 pending, 2 skipped)
0m00.001s
Cucumber found our step definitions and executed them. They are currently marked as pending, which means we need to make them do something useful.

# STEP 3.1 See scenario reported as failing 🔗︎
The next step is to do what the comments in the step definitions is telling us to do:

Write code here that turns the phrase above into concrete actions

Try to use the same words in the code as in the steps.

--Ubiquitous Language

If the words in your steps originated from conversations during an Example Mapping session, you’re building a Ubiquitous Language, which is a great way to make your production code and test easier to understand and maintain.

Change your step definition code to this:

const assert = require('assert');
const { Given, When, Then } = require('cucumber');

function isItFriday(today) {
  // We'll leave the implementation blank for now
}

Given('today is Sunday', function () {
  this.today = 'Sunday';
});

When('I ask whether it\'s Friday yet', function () {
  this.actualAnswer = isItFriday(this.today);
});

Then('I should be told {string}', function (expectedAnswer) {
  assert.equal(this.actualAnswer, expectedAnswer);
});
Run Cucumber again:

..F

Failures:

1) Scenario: Sunday is not Friday # features/is_it_friday_yet.feature:4
   ✔ Given today is Sunday # features/step_definitions/stepdefs.js:8
   ✔ When I ask whether it's Friday yet # features/step_definitions/stepdefs.js:12
   ✖ Then I should be told "Nope" # features/step_definitions/stepdefs.js:16
       AssertionError [ERR_ASSERTION]: undefined == 'Nope'
           at World.<anonymous> (/private/tmp/tutorial/hellocucumber/features/step_definitions/stepdefs.js:17:10)

1 Scenario (1 failed)
3 steps (1 failed, 2 passed)
That’s progress! The first two steps are passing, but the last one is failing.

# STEP3.2 See scenario reported as passing
Let’s do the simplest possible thing to make the scenario pass. In this case, that’s simply to make our function return Nope:

function isItFriday(today) {
  return 'Nope';
}
Run Cucumber again:

...

1 Scenario (1 passed)
3 steps (3 passed)
0m00.003s
Congratulations! You’ve got your first green Cucumber scenario.