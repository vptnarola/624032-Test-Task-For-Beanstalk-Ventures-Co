# FinTech (fintech)
FinTech Project

### Dependencies
  1. Quasar CLI ( as we are using Quasar Vue js Framework )
  2. Json-server ( https://www.npmjs.com/package/json-server )
  3. Axios 

## Install the other npm dependencies
```bash
yarn
# or
npm install
```
### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```
### Lint the files
```bash
yarn lint
# or
npm run lint
```
### Format the files
```bash
yarn format
# or
npm run format
```
### Build the app for production
```bash
quasar build
```
### Customize the configuration
See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-webpack/quasar-config-js).

### Details of implementation ( Test task: 2 Sample from Scratch )
- Reference snapshot of the Application: https://prnt.sc/A11u45hr_mNI , https://prnt.sc/Q0nq8zeF6SBv
1. As we have using json-server to integrate backend for fake data, so before execute project please install it by fire this command "npm install --location=global json-server"
2. npm install
3. quasar dev ( require to execute project )
4. Task Details: user can register, create a property, and enroll in other users' properties.
5. Implementation: 
- Here, we have use component and layout from the core Quasar component
- About the first page, user can see Login & Singup button on header section and here users can see the list of properties, listed by other user available on the system.
- User can register themsleves by SignUp feature with his/her valid email address, as here we have perfom frontend validation as well
- After successful Login, user can Login into the system with the same credentials. Please make a note that with incorrect login credentials users can not login into the system
- Once they logged in successfully, user will land into the same landing page... but they can noticed changes into header section as they are logged in to the system, they can now able to add properties into system by the simple form, and beside it they have logout button into the top-right corner of the page to logout from the system
- Here, on logged in attempt as we are using json-server apis, we are validating users from the db.json file( here, data is stored already on his/her registration process )... and once thier data matched we are storing user session by local storage in the browser
- About Add Property, we have open the form into right drawer, where user can add basic property details such as title, location of country ( by country, city etc), property details and to make more userfriendly we have add no. of bedrooms, no. of bathrooms, no. of bedrooms fields as well. Here, we have added image upload fields but due to backend logic the selected image by users which are just for reference, and while submitting "add property" form system will take the static images itself. ( with backend logic we can make it dynamic, but as its not in test task requirement we have skip it for now )
- Also, we have perform frontend validaiton as well into "Add Property" form
- Addtionally, after logged in to they system, users will have "Book" button to book the property available into the system 
- Here, users can book multiple/same property multiple times with various date of selection.
- About "Logout", we are cleaning data from the Local storage of the browser while users are logging out from the system and after that user will land into default landing page
