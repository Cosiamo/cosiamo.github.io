> # Table of Contents
>> ## Work Processes
>>> ### Principles and Practices
>>>> ##### Use and contribute to shared and open GitHub repositories
>>>> ##### Gather data and conduct analysis to draw insights
>>> ### Software Engineering Fundamentals
>>>> ##### [Perform software testing and problem solving](#software-testing-with-jest) 
>>>> ##### [Perform system scaling and security](#system-scaling-and-security-with-enzyme-and-jest)
>>> ### DevOps
>>>> ##### Perform continuous integration of code
>>>> ##### Perform continuous delivery of code
>>>> ##### Implement metrics and measurement
>>> ### Platforms, Services, and Solutions
>>> ##### Use logging and monitoring tools

>> ## Competencies 
>>> ### DevOps
>>>> ##### Understand and demonstrate DevOps automation
>>>> ##### Understand and demonstrate DevOps metrics and measurements
>>>> ##### Understand and demonstrate continuous delivery
>>>> ##### Understand and demonstrate continuous integration
>>>> ##### Understand and demonstrate feature decoupling
>>> ### Platforms, Services, and Solutions
>>>> ##### Understand and demonstrate knowledge of cloud computing fundamentals, including the variuos tools, services and principles
>>>> ##### Understand and demonstrate the design patterns and practices for building cloud native services
>>>> ##### Understand and demonstrate the use of logging and monitoring tools and infrastructure
>>>> ##### Understand the need for mobility and migration of data from on-premise to cloud solutions, and the implications
>>>> ##### Understand the relationship between scaling techniques, how to exploit them, and where
>>>> ##### Understand the topologies of enterprise solutions, and how clients use our portfolio of products and services together
>>>> ##### Understand the various platforms, their differences, relative strengths / weaknesses, and integrations
>>> ### Principles and Practices
>>>> ##### Demonstrate ability to analyze data sets, identify insights, and leverage to drive decision making
>>>> ##### Demonstrate key teamwork and collaborative behaviors
>>>> ##### Demonstrate strong communication skills
>>>> ##### Understand and demonstrate social coding behaviors
>>>> ##### Understand and model good feedback behaviors
>>>> ##### Understand, articulate, and demonstrate agile principles and practices
>>>> ##### Understand, articulate, and demonstrate IBM Design Thinking
>>> ### Software Engineering Fundamentals
>>>> ##### Demonstrate knowledge of key computer programming fundamentals
>>>> ##### Understand and demonstrate how to build and test quality code, at scale
>>>> ##### Understand and demonstrate key software design fundamentals 
>>>> ##### Understand and demonstrate knowledge of web programming skills 
>>>> ##### Understand and demonstrate test-driven development
>>>> ##### Understand and manage technical debt
>>>> ##### Understand and navigate the complexity associated with enterprise-level devlopment
>>>> ##### Understand how to use version control for all elements of the software delivery lifecycle
>>>> ##### Understand, articulate, and demonstrate clean coding behaviors

---

## Software Testing with **Jest**

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

## System Scaling and Security with Enzyme and Jest

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

