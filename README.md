# Apprentice Database Submissions from Joshua Becnel
This site was created by Joshua Becnel to submit the necessary requirements in order to complete the Apprentice Database (Application Development Specialist - Work Processes & Competencies). I have already submitted some of the dependencies before I started working on this site, that's why some will be missing from the Table of Contents. To view specific submissions, click the corresponding links in the Table of Contents.

> # Table of Contents
>> ## Work Processes
>>> ### Principles and Practices
>>> - Use and contribute to shared and open GitHub repositories
>>> - Gather data and conduct analysis to draw insights
>>> 
>>> ### Software Engineering Fundamentals
>>> - [Perform software testing and problem solving](#software-testing-with-jest) 
>>> - [Perform system scaling and security](#scaling-applications-with-apis-and-how-to-secure-them)
>>> 
>>> ### DevOps
>>> - Perform continuous integration of code
>>> - Perform continuous delivery of code
>>> - Implement metrics and measurement
>>> 
>>> ### Platforms, Services, and Solutions
>>> - Use logging and monitoring tools

>> ## Competencies 
>>> ### DevOps
>>> - [Understand and demonstrate DevOps automation](#devops-automation-and-feature-flags)
>>> - [Understand and demonstrate DevOps metrics and measurements](#metrics-and-measurements)
>>> - [Understand and demonstrate continuous delivery](#continuous-delivery)
>>> - [Understand and demonstrate continuous integration](#continuous-integration)
>>> - [Understand and demonstrate feature decoupling](#feature-decoupling)
>>> 
>>> ### Platforms, Services, and Solutions
>>> - Understand and demonstrate knowledge of cloud computing fundamentals, including the various tools, services and principles
>>> - Understand and demonstrate the design patterns and practices for building cloud native services
>>> - Understand and demonstrate the use of logging and monitoring tools and infrastructure
>>> - Understand the need for mobility and migration of data from on-premise to cloud solutions, and the implications
>>> - Understand the relationship between scaling techniques, how to exploit them, and where
>>> - Understand the topologies of enterprise solutions, and how clients use our portfolio of products and services together
>>> - Understand the various platforms, their differences, relative strengths / weaknesses, and integrations
>>> 
>>> ### Principles and Practices
>>> - Demonstrate ability to analyze data sets, identify insights, and leverage to drive decision making
>>> - Demonstrate key teamwork and collaborative behaviors
>>> - Demonstrate strong communication skills
>>> - Understand and demonstrate social coding behaviors
>>> - Understand and model good feedback behaviors
>>> - Understand, articulate, and demonstrate agile principles and practices
>>> - Understand, articulate, and demonstrate IBM Design Thinking
>>>
>>> ### Professional Skills Development
>>> - [Complete Ways of Thinking module in Apprenticeship Skills Launchpad](#ways-of-thinking-module)
>>> - [Complete Ways of Working module in Apprenticeship Skills Launchpad](#ways-of-working-module)
>>> 
>>> ### Software Engineering Fundamentals
>>> - Demonstrate knowledge of key computer programming fundamentals
>>> - [Understand and demonstrate how to build and test quality code, at scale](#test-and-scale-code-with-enzyme-and-jest)
>>> - Understand and demonstrate key software design fundamentals 
>>> - Understand and demonstrate knowledge of web programming skills 
>>> - [Understand and demonstrate test-driven development](#test-driven-development)
>>> - Understand and manage technical debt
>>> - Understand and navigate the complexity associated with enterprise-level devlopment
>>> - Understand how to use version control for all elements of the software delivery lifecycle
>>> - [Understand, articulate, and demonstrate clean coding behaviors](#understand-articulate-and-demonstrate-clean-coding-behaviors)

---

# Software Testing with **Jest**

Jest is a powerful tool, created and maintained by Facebook, for testing JavaScript code.
A few important things it does are:
- Tests asynchronous code
- Snapshot testing
- Create Mock functions
- Exceptions (Jest methods) that have great syntaxing
	- .toBe()
	- .toBeCloseTo()
	- .toEqual()
	- .toMatchSnapshot()
- and much more...

Jest offers a set of global variables, the most important one being `test`.
>Test requires 2 arguments:
>- A name (string) that describes the test case
>- Code to run for the test case

#### Example:
```javascript
const add = (a,b) => a + b;

test('Should add 2 numbers', () => {
    const result = add(3,4);
    if (result !== 7) {
        throw new Error(`The result was ${result}. Was expecting 7.`)
    }
});
```

This is a passing test case because the function "add" was given the arguments of 3 and 4, while 7 was the expected result.

However, if "add" was:
```javascript
const add = (a,b) => a + b + 1;
```
Then the test would fail with the error message `The result was 8. Was expecting 7.` in the terminal.

Jest also provides an assertion library. Assertion functions allow you to make assertions about values in the program. One of the most important assertion functions is *expect()*. With expect() you need to pass in the variable as an argument, then access a Jest method.

To show how it works I'll redo the previous example using an assertion function and a Jest method
```javascript
const add = (a,b) => a + b;

test('Should add 2 numbers', () => {
    const result = add(3,4);
    expect(result).toBe(7)
});
```

The assertion function and Jest method basically replace the "if statement".

---

# Scaling Applications with APIs and How to Secure Them

One of the best ways to quickly scale your application is to integrate APIs (application programming interface). They can do things such as:
- Adding weather data
- Enabling two-factor authentication for your users
- Add real-time hotel listings
- Use a device's built-in camera
- and much more...

Most third-party APIs have the ability to let you make them more dynamic.
- You can inject your own variables into the URL
- Can have their own functions, objects, or properties you can call

For example, if you're adding weather data in a Node.js project with the WeatherStack API: 
```javascript
const url = 'http://api.weatherstack.com/current?access_key=[YOUR_ACCESS_KEY]&query=newyork&units=f'
```

>Let's breakdown `http://api.weatherstack.com/`
>- `current` refers to the time-frame you wish to pull the data
>- `?access_key=` is verification to prove that you're the one calling the API from WeatherStack
>>- This is important because usually APIs charge you for a certain amount of uses 
>>- Don't share this key or you will be charged a ton of money if someone uses the API with your access key
>>- Typically a randomly generated string
>- `&query=` sets the location
>- `&units=` sets the weather temp. to Fahrenheit or Celsius

If you're sharing your code on an open repository like GitHub, you will need to secure your *access key*. The best way to do that is to make your key an environment variable listed in an `.env` file.
```env
WEATHERSTACK_API_KEY=[YOUR_ACCESS_KEY]
```
It's a best practice to add `.env` files to a folder labeled `config` and add that folder to your `.gitignore` file. In order to call the environment variable, you would use the syntax `process.env.{variable name}`.

To see the how this API works in code using your own variables, WeatherStack's dynamic properties, and calling a secure access key from an `.env` file:
```javascript
const request = require ('request');
const apiKey = process.env.WEATHERSTACK_API_KEY;
// latitude and longitude of Baton Rouge
const lat = 30.471165;
const long = -91.147385;
const forecast = (lat, long, apiKey) => {
    const url = 'http://api.weatherstack.com/current?access_key=' + apiKey + '&query=' + lat + ',' + long + '&units=f';
	request({url, json:true} => {
	    const {temperature, feelslike, humidity, uv_index} = current;
		const t = temperature;
		const fl = feelslike;
		const h = humidity;
		const uv = uv_index;
		console.log('It is currently ' + t + ' degrees out. It feels like ' + fl + ' degrees. The humidity is ' + h + '% and the UV index is ' + uv + '.')
	})
};
```
The end result will print the sentence from `console.log` with the up-to-date temperature, feels-like temp., humidity, and UV index from WeatherStack's API. With some random example data, it would look like:
```
It is currently 90 degrees out. It feels like 95 degrees. The humidity is 30% and the UV index is 4.8.
```

**Summary of the example code**
- The variables `apiKey`, `lat`, and `long` were all injected into the URL to make the API secure and dynamic
- Destructured `current` to get specific properties 
	- Side note: it's typically not a good idea to abbreviate variables in larger projects, I just did it for this example to make the sentence in `console.log` a little more readable
- Injected the pre-defined, dynamic properties of `current` into the sentence that we want printed to the console

---

# DevOps Automation and Feature Flags
DevOps Automation is the implementation of technology that performs tasks and operations with limited human intervention. The main objectives are to accelerate development, scale applications quickly, as well as build [continuous integration](#continuous-integration) and [continuous delivery](#continuous-delivery) workflows.

One way to automate your workflow is to implement feature flags. **Feature Flags**, aka Feature Toggles or Switches, is a method used to deliver and integrate code without re-deploying the application. It allows you to turn features on or off whenever you need to based on parameters you set. You can set these up manually by adding properties in `.json` files or config maps. However, it's much easier to to use a feature flag service, such as Split or Flagsmith. Services, such as these, offer:
- a central place to manage your flags
- can turn them on or off without modifying features in your apps
- easier to audit and get usage data than with `.json` files

**Benefits of Feature Flags:**
- can turn them on or off without deployment
- can test directly in production
- segment users based on different attributes

Segments are group of users that have attributes tied to them.
- current location
- user zip code
- admin privileges
- email ID
- etc..

Segmenting your users based on specific parameters allows you to Dark Launch new features in your application. A Dark Launch is when a feature is in production but not visible to any or some users.

---

# Metrics and Measurements
**DevOps Metrics** are data points that track the progress of a software development pipeline. The fast feedback from these data points helps identify and address any bottlenecks in the development life-cycle.

>Four main DevOps metrics
>- Lead time for changes
>	- The amount of time it takes for work to be completed
>- change failure rate
>	- The ratio of working and broken features
>- Deployment frequency
>	- The amount of builds that makes it to your user base
>- Mean time to recovery (MTTR)
>	- The duration of time an outage is detected to when it is repaired

>The best way to measure these data points are with these simple equations:
>- Lead time for changes
>	- `Time deployed to production - the start time`
>- change failure rate
>	- `(Failed deployments/total deployments * 100)`
>- Deployment frequency
>	- `(Successful deployments/total deployments * 100)`
>- Mean time to recovery (MTTR)
>	- `Resolved timestamp - start timestamp`

---

# Continuous Delivery

New versions of software being released used to be a bottleneck for the teams working on applications. Manual testing as well as operation and development teams working in silos resulted in unreliable releases, overlooked errors, and delays. Automating version releases allows teams to *fail fast*. In other words, tests that are most likely to fail are ran first and tests that take longer to run only happen after the faster ones are completed. This is where **Continuous Delivery** comes in.

Continuous Delivery is a discipline where a program, application, or any other software can be released to production at any time. 
- Programmers, IT infrastructure team, UI/UX designers, and the operation teams should communicate and work together
	- Everyone has access to the code
	- Everyone can see everything
- Automate releases and tests
- Have a proper and universal understanding of what needs to be done
- Need to have the infrastructure for [continuous integration](#continuous-integration) in place

The four key principles of Continuous Delivery are:
- Continuous Exploration
- Continuous Deployment
- Release on Demand
- [Continuous Integration](#continuous-integration)

---

# Continuous Integration

Continuous Integration is a development practice that asks developers to integrate code into a shared repository everyday.
- All contributors make a commit to the main branch everyday
- Automate the process of building and testing 
- Have a single, central place to commit code
	- An example of this would be a **monorepo** (monolithic repository)

A monorepo is a large, single repository that holds many different projects that are all version controlled. These projects are can be independent of each other and ran by different teams. Many large companies such as Google, Microsoft, and Twitter have monorepos. It is thought that Google has the largest code repository ever with over 80TBs of data. These are great for continuous integration because they offer:
- Greater visibility
- Easier dependency management
- A unified build process
- Consistency
- And much more...

Continuous integration is fast and takes less effort from the developers. Issues will be more apparent which makes them faster to catch and fix. The automation process means there are less issues that you have to deal with. Because everyone is contributing to the repository everyday, that means:
- Better communication between contributors 
- Shorter time between integration iterations
- The code base is ready to be delivered much faster

---

# Feature Decoupling


It is important to decouple the functionality of your features for many reasons. For new features, so that Class.X doesn't break whenever you're adding Class.Y. If you tightly couple the functionality of an existing class (Class.X) with a new class (Class.Y), the chances of Class.X being affected when Class.Y is changed are high. It's a good idea to decouple existing features as well. 
- it's easier to add and improve functionality
- better for the overall security of your application

One benefit that Feature Flags offers is the ability to segment your user base. This allows you to decouple specific features between certain users, administrators, and developers. By doing this you're not only doing feature decoupling for functionality and security, but by making the program more dynamic from user-to-user as well.

Examples:
- allow users that sign up for beta testing to try out new features before they're toggled on for the rest of the user base
- show a "local specialties" menu for a restaurant chain, that has multiple locations, to users that share the zip code to a specific restaurant location
- allow access to view certain information based on admin privileges

Let's say you were creating a website with React JS for a restaurant. They want the menu page to only display the dishes to their regular customers. However, for their loyal customers they not only want to display the dish, but also the recipe for it. 
You would set it up to look something like: 

A file housing customer info
```javascript
const customerType = {
	isLoyalCustomer: true
};
```

A file housing recipe info
```javascript
const recipeFrenchOnionSoup = "The recipe is..."
```

A file for the Higher Order component
```javascript
const requireAuthentication = (WrappedComponent) => {
	return (props) => (
		<div>
			{props.isLoyalCustomer ? (<WrappedComponent {...props} />) : (<WrappedComponent />)};
		</div>
	);
};
```

And a file for the menu
```javascript
const Menu = (props) => (
	<div>
		<h1>Menu</h1>
	
		<p>French Onion soup</p>
		<p>{props.recipe}</p>
	</div>
);

const CustomerStatus = requireAuthentication(Menu);
ReactDOM.render(<CustomerStatus customerType recipe={recipeFrenchOnionSoup} /> document.getElementById('app'));
```

The advantages to this scenario:
- The functionality of checking if the user is a loyal customer isn't tightly integrated in `Menu`
	- The ternary operator `{props.isLoyalCustomer ? (<WrappedComponent {...props} />) : (<WrappedComponent />)}` makes it so that the `Menu` will still render even if you're changing how `isLoyalCustomer` works 
- `customerType` makes the experience of the site more dynamic from user-to-user
- The functionality, customer info, and the recipe info are their own variables which makes it easier to [scale the application](#continuous-delivery)
- Everything is separated into their own files so that the object is unaware of the logic behind the toggling decision

---

# Ways of Thinking module
> ## Business Acumen
> On the week of Fourth of July I took the “Core Capabilities Fundamentals” class in which we learned the best practices in sales meetings. The class was divided into teams and we were tasked with creating a pitch for a fictional telecommunication merger for the end of the week. We got together on WebEx for the next few days creating a Power Point of our proposal, practicing our presentations, and building team chemistry. We used business acumen to create realistic scenarios for what work would have to entail as well as what the time table would look like in order for the project to be completed. At the end of the week our team ended up winning the competition.
> ## Creative Thinking and Innovation
> The group of other apprentices I talk with everyday and I need experience contributing to a group coding project for the apprentice data base and since we haven’t been told when we’re shadowing any real projects we decided to do a group project on our own. We are actively brainstorming what we want to do for the project but we’ve narrowed it down to making the periodic table of elements with a front end showing the table and a SQL database that lists the properties of each element. The reason we’ve decided this particular project is because we need to complete the specific competencies “Demonstrate key teamwork and collaborative behaviors” and “Understand and demonstrate social coding behaviors”. Because of this we are restricted to certain criteria that we need to complete. Those are: demonstrate successful use of pair programming, demonstrate successful use of playbacks and reviews, demonstrate knowledge of social coding behaviors, execute successful ‘Fork and Pull’ model in GitHub, understand the use of Open Source and the implications, and a few more that are similar. Those restrictions are helping us in our creativity of what we plan on accomplishing through this project. That is how we came to the result of creating the periodic table with those criteria in mind in every step of the process.
> ## Critical Thinking and Problem Solving
> One situation where I applied critical thinking skills in the workplace was when I was working for AT&T and I had to find a way to boost business sales for our store in a very competitive market. I first asked myself, “why are business sales down?” And the answer was because this was the fourth AT&T store they opened in a 10 mile radius. Then I asked “why don’t we find a way to differentiate ourselves from the stores?” So the answer was clear and simple, find a way to make us stand out. The follow up question to that was “what are some ways we can do that?” Well, if the customers weren’t coming to us, we should go to the customers. “Where can we meet the customers?” We decided that we should go to businesses’ locations and events. And finally, “why would they choose us to help them?” After contemplation we decided that the best way to do that was to have un-rivaled customer support. Be there for their every question they have and help them with device transfers, smartwatch setups, and Bluetooth setups. The result was that we boosted our store’s business sales by over 130% from the prior month.
> ## Decision Making
> When I was working in mobile sales, I would frequently encounter fraud attempts. What that means is that a customer would use a fake ID, use someone else’s account, or have another person that doesn't care about their credit score to take out a lease on a phone they don’t plan on paying. One example of this situation is, when I was working as a third-party representative for AT&T at Best Buy, we had an older man claim he was opening a new account to get two brand new iPhones for his daughters. We had different ways of identifying potential fraud and he was checking a few the boxes (ID had red font on the top right saying it wasn't for federal use and he was just asking about the initial down payment and didn’t ask anything about the monthly costs). However, you can’t flag someone as fraud without obtaining more information because you could be losing a legitimate customer if you’re wrong. So, the task at hand was to decide if I should flag the account or proceed with the sale. The actions I took were asking some questions I’ve come up with to narrow down if it’s legitimate, such as: “Do you need help transferring data or setting up the devices?”, “Do you want to know about the features and prices between the different plans we offer?”, and “Are you sure you don’t want to add your number to this new account? It’ll be the third line which gives you a multi-line discount that’s cheaper and has better features than your current plan.”. Those questions sound like basic questions you would hear at a cell phone store and that’s the point, you don’t want to offend the customer with blunt questions if they are legitimate and you gain valuable information into proceeding with the sale. The reasons you would use those questions in particular is because people committing fraud don’t want you setting up the phone(s) because they plan on selling them as unopened/brand new, they don’t care about the features or prices of the plans because they never plan on paying or using the lines, and they don’t want to move their personal number on the account even if it saves them money and/or offers better features because they know it’ll link them personally to the fraudulent account. This customer’s answers to the questions raised more concerns with me because it was seeming more likely that this was fraud. The result of this interaction made me go tell the mobile sales supervisor all the information I’ve gathered and she deemed it as fraudulent so we flagged the account and came up with a polite way of telling him that the carrier won’t let us proceed with the transaction and that he would have to contact AT&T directly. If the sale had gone through our store would have had to absorb the cost of two iPhone 12 Pro Maxes worth $1449.99 each.

---

# Ways of Working module
> ## Adaptability and Flexibility
> One time I had to adapt and be flexible in the workplace was when I was working at Boost Mobile and I was promoted to lead repair tech of a different store location and I had no idea about it until I clocked in that day. The task at hand was to set up a repair shop in the back of the store that never had one. In order for me to succeeded at this task I needed to start from the ground up and communicate daily with the franchise owner. I had to get a table, new tools from iFixit, and get parts from our supplier. I adapted to the new store quickly and started repairs almost immediately. I also was able to train a few people in the process. 
> ## Being a Trusted Advisor
> I had to build trust with my coworkers when I got hired at IBM. None of us knew each other before we were all hired here so we had to build trust among each other. The first big situation we had to build trust was during the “Ehub School June 2021” from June 14th-25th. We knew each other for only 3 weeks at that point and this was the first class our group took together. After the classes we would get together on WebEx calls or message each other on Slack to share what we retained from class that day. Along the way we started to know each other better and started to trust one another’s input on the different topics and discussions. The result of this was that we now trust each other’s intuition and that has helped us tremendously throughout this apprenticeship.
> ## Leadership
> One time I showed leadership in the workplace was when I was working for Boost Mobile. The franchise owner moved our store manager to another location without appointing a new manager at our store, so I stepped up to fill the role. I would manage our store’s inventory, set up hours for my coworkers and I, and reported sales results to our franchise owner and Sprint rep. I even trained a new repair technician. Even though our store’s volume was always low, I still was able to keep it afloat and meet monthly goals while I was filling the role without any proper manager training whatsoever.
> ## Teamwork and Collaboration
> I actively practice teamwork and collaboration daily with Johnnie Gonzalez, Matthew Crain, and Skyler Pointer. Every day we talk with each other on Slack to see where we are in the apprentice launchpad and apprentice database. We answer each other’s questions and help with problems that any of us have. We set up WebEx meetings every so often so we can collaborate and plan our next steps in order to complete the apprenticeship as soon as possible. The result has been that we’ve completed a large chunk of what we’ve been assigned and gained a plethora of knowledge regarding various coding languages, proper coding syntax, and a better understanding of SAP and all of the intricacies in their partnership with IBM. 

---

# Test and Scale Code with Enzyme and Jest

One if the problems with scaling an application is that the larger it becomes, more vulnerabilities and bugs can be overlooked. For example, if you're creating accounting software with React JS, the most common testing tool is Jest but it doesn't scale well whenever you need to test multiple components. That's where Enzyme comes in. It was created and still maintained by Airbnb for the purpose of scaling out testing in React applications and to work along side Jest.

So, in that accounting software that was previously mentioned, if you wanted to add functionality so that the user is entering a valid dollar amount you would write something like: 

```javascript
onAmountChange = (e) => {
    const amount = e.target.value;
    if (!amount || amount.match(/^\d{1,}(\.\d{0,2})?$/)) {
        this.setState(() => ({amount}));
    }
};
```

The important part for validating that the user input was a dollar amount (only numbers that have a maximum of 2 decimals) and not an invalid number, string, or even malicious code is the regex; `/^\d{1,}(\.\d{0,2})?$/`. Now let's say you're scaling the capabilities of your application and now there are multiple inputs.
- One for the dollar amount
- One for a description

```html
<form onSubmit={this.onSubmit}>
    <input
	type="text"
	placeholder="Description"
	autoFocus
	value={this.state.description}
	onChange={this.onDescriptionChange}
    />
    <input
	type="text"
	placeholder="amount"
	value={this.state.amount}
	onChange={this.onAmountChange}
    />
```



How would you test that the user is indeed entering a valid dollar amount in the input as your application grows in complexity? If you're scaling the application even more, how would you know that any future input was properly working or you didn't accidentally break the `onAmountChange` function?

You would need to have shallow rendering to test the component as well as assertion functions and methods from Jest. Thankfully Enzyme has a function for shallow rendering that is called `shallow`. For shallow you simply call the component as the argument. 

```javascript
test('Should set amount if valid input', () => {
    const value = '23.50'
    const wrapper = shallow(<ExpenseForm />);
    wrapper.find('input').at(1).simulate('change', {
	target: {value}
    });
    expect(wrapper.state('amount')).toBe(value);
});

test('Should NOT set amount if invalid input', () => {
    const value = '12.122'
    const wrapper = shallow(<ExpenseForm />);
    wrapper.find('input').at(1).simulate('change', {
	target: {value}
    });
    expect(wrapper.state('amount')).toBe('');
});
```

Now both of these test cases can make sure that the user is indeed providing the correct value in the input. 

- The variable `value` is the user input we want to test
- `shallow` is calling the component that we want to test
- `.find('input')` is looking for an input tag in the component
- `.at(1)` is searching for the specific input tag that we want to test
	- because there are 2 inputs, we want to test the one that calls for `onAmountChange`. Since that is the second input tag, we pass the index of 1 as the argument
- `.simulate('change', { target: {value} });` simulates the changing the the target value with the `value` variable that was defined.

This method makes it much easier to test the functionality of the code as the application grows larger and becomes more complex.
---

# Test Driven Development

Test Driven Development (TDD) is the practice of writing out test cases first, then writing code to pass those test cases. It is sometimes referred to as the Red-Green-Refactor method (fail-pass-refactor). In other words, it's a technique where you describe the behavior of the code you want to write, as a test case, then implement it into the project.

There are different types of testing. The three main types are:
- Unit testing
	- adds value to a code base at the lowest level
	- validates the behavior of individual functions, methods, or units of code
- Integration testing
	- Testing multiple units of code together
	- Can write unit tests to test each unit of code then write an integration test to see how well they work together
- End-to-end testing
	- runs the app in a simulated environment
	- attempts to emulate actual user behavior

Other types of testing: 
- Acceptance testing
	- test client or user's requirements
- System testing
	- ensures everything works on real hardware
- Sanity/Smoke testing
	- runs the important tests first, then everything else
- Functional Testing
	- tests actual code
- non-functional Testing
	- Performance
	- usability 
	- security
- stress-testing/fail-over testing
	- used to test the capabilities of the infrastructure as opposed to the code itself

If we were creating an accounting application and we wanted to have a function that calculated the total amount of each dollar amount the user inputs, we would need to first write out what we want the function to accomplish. It would need to return 0 if there aren't any expenses, return a single amount if the user only inputs a single expense, and add all expenses if the user inputs multiple.

*(All of this example code is written with JavaScript and being tested using Jest)*

- First we need fixture data for the test cases
```javascript
export default [{
	id: '1',
	description: 'Gum',
	note: '',
	amount: 195,
}, {
	id: '2',
	description: 'Rent',
	note: '',
	amount: 109500,
}, {
	id: '3',
	description: 'Credit Card',
	note: '',
	amount: 4500,
}];
```

- Once we have the fixture set, we write a test case that expects 0 
```javascript
test('Should return 0 if no expenses', () => {
	const result = selectExpensesTotal([]);
	expect(result).toBe(0);
});
```

- There isn't anything to test yet so it will fail
- Since we have only have a test that expects 0 we only need to write the function to return 0 for now
```javascript
export default (expenses) => {
	if (expenses.length === 0){
		return 0;
	} else {

	}
};
```

- Now that there is a function that returns 0 the test will pass
- We can now test for the user inputting an amount with the fixture data
```javascript
test('Should correctly add up a single expense', () => {
	const result = selectExpensesTotal([expenses[1]]);
	expect(result).toBe(109500);
});
test('Should correctly add up multiple expenses', () => {
	const result = selectExpensesTotal(expenses);
	expect(result).toBe(114195);
});
```

- Again, we should expect the tests to fail because the function isn't set up yet
- Because of the fixture data that was set up for these tests we can expect the expense with the index of 1 to be 109500 and all of the expenses being added together to be 114195
- Now we just need to complete the function
```javascript
export default (expenses) => {
	if (expenses.length === 0){
		return 0;
	} else {
		return expenses
			.map((expense) => expense.amount)
			.reduce((sum, value) => {
				return sum + value;
			}, 0);
	}
};
```

- The test will pass because the function adds the expenses together 

---

# Understand, articulate, and demonstrate clean coding behaviors

There are many reasons why you should write clean code, the most important reason is the readability of your code. Reading code can be much more difficult than writing code. Whether it's your coworkers, contributors to your open source project, or even yourself if your're refactoring code that you originally wrote weeks ago.

#### There are few techniques to writing cleaner code:

- Naming Conventions
	- Decide how you'll name your variables and stick to a set of rules.
	- Keep it simple, make your variables understandable to other people.
- Don't repeat yourself
- Document code
	- You don't have to just add a `README.md` or `README.txt` file in order to document your code. You can also add comments or even let the code "document itself" by having variable names properly describe what they're doing (refer back to naming conventions).
- Avoid abbreviations
- When in doubt, follow the **SOLID** principle
	- Created by *Robert C. Martin* in order to to have a coding standard for Object-Oriented Programming. It stands for:
		- **S.**olid responsibility principle
		- **O.**pen Closed Principle
		- **L.**iskov substitution principle
		- **I.**nterface segregation principle
		- **D.**ependency inversion principle

**Example of sloppy code:**
```javascript
const thing = {
    num: [10, 20, 30],
    otherNum: 2,
    thing() {
	return this.num.map((num) => num * this.otherNum);
    }
};
console.log(thing.thing());
```

Even though this code works, both `thing` variables have no real meaning and the name repeats itself which makes it difficult for other people to understand. The array with the variable name `num` is the exact same syntax as the value 'num', which can cause confusion when reading `this.num.map((num) => num`. What does `otherNum` even do?

**Previous example but written clean coding techniques:**
```javascript
// multiply an array of numbers by 2
const multiplier = {
    numbers: [10, 20, 30],
    multiplyBy: 2,
    multiply() {
	return this.numbers.map((num) => num * this.multiplyBy);
    }
};
console.log(multiplier.multiply());
```

This code works the exact same way as the previous example, however, it's much easier to understand what is supposed to do just by simply changing the syntax of the variable names. 
- The comment leaves a clear note of what this block of code is meant to do
- The variable names `mutiplier` and `multiply` imply what they're supposed to do and have two distinct names to avoid confusion
- `numbers` gives a clear distinction from 'num'
- `multiplyBy` implies that this is the variable that is being multiplied by the array
