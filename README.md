## setup cypress, cucumber with angular application
create new angular application with
***
ng new <app-name>
***
cd in to <app-name> folder  
check the angular application running by
***
yarn start  
***
install cypress with
npm install -D cypress

run the cypress with
yarn cypress open

add the below in the cypress.json config file

{
  "baseUrl": "http://localhost:4200",
  "ignoreTestFiles": "**/examples/*",
  "viewportHeight": 760,
  "viewportWidth": 1080
}


install cypress-cucumber-preprocessor with
npm install --save-dev cypress-cucumber-preprocessor

add the below code in cypress/plugins/index.js

const cucumber = require('cypress-cucumber-preprocessor').default

module.exports = (on, config) => {
  on('file:preprocessor', cucumber())
}

then add the below line in cypress.json
{
  "testFiles": "**/*.{feature,features}"
}

optional to run the bundled tests use

yarn cypress run --spec **/*.features 

Add the below to package.json

"cypress-cucumber-preprocessor": {
  "nonGlobalStepDefinitions": true
}

add app.feature file under cypress/integration folder

try the below specs

Feature: Go to the login page
  Display the title


  @focus  
  Scenario: Login page title
    Given I visit login page at "http://localhost:4200/"
    When I do nothing
    Then I should see "UserAdminUi" as "title"

  @focus
  Scenario: Learn angular element
    Given I visit login page at "http://localhost:4200/"
    When I do nothing
    Then I should see "Learn Angular" as "span"

add app folder under cypress/integration
add app.spec.js step file under cypress/integration/app folder and add the below steps


import { Given } from "cypress-cucumber-preprocessor/steps";

Given(`I visit login page at {string}`, (url) => {
    cy.visit(url)
})
When(`I do nothing`, () => {
    return 'success'
})
Then(`I should see {string} as {string}`, (value, element) => {
    // cy.title().should('contain', title)
    cy.get(element).should("contain", value)
})
 

Integrate cypress with angular: https://indepth.dev/posts/1349/write-better-automated-tests-with-cypress-in-angular

Integrate crypress with cucumber: https://www.npmjs.com/package/cypress-cucumber-preprocessor
