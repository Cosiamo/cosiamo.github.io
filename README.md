# Apprentice Database Submissions from Joshua Becnel
This site was created by Joshua Becnel to submit the necessary requirements in order to complete the Apprentice Database (Application Development Specialist - Work Processes & Competencies). I have already submitted some of the dependencies before I started working on this site, that's why some will be missing from the Table of Contents. To view specific submissions, click the corresponding links in the Table of Contents.

> # Table of Contents
>> ## Work Processes
>>> ### Principles and Practices
>>> - [Use and contribute to shared and open GitHub repositories](#social-coding-behaviors-and-shared-github-repository)
>>> - [Gather data and conduct analysis to draw insights](#gather-data-and-conduct-analysis-to-draw-insights)
>>>
>>> ### Software Engineering Fundamentals
>>> - [Perform software testing and problem solving](#software-testing-with-jest)
>>> - [Perform system scaling and security](#scaling-applications-with-apis-and-how-to-secure-them)
>>>
>>> ### DevOps
>>> - [Perform continuous integration of code](#work-process)
>>> - [Perform continuous delivery of code](#work-process)
>>> - [Implement metrics and measurement](#metrics-and-measurements)
>>>
>>> ### Platforms, Services, and Solutions
>>> - [Use logging and monitoring tools](#logging-and-monitoring)

>> ## Competencies
>>> ### DevOps
>>> - [Understand and demonstrate DevOps automation](#devops-automation-and-feature-flags)
>>> - [Understand and demonstrate DevOps metrics and measurements](#metrics-and-measurements)
>>> - [Understand and demonstrate continuous delivery](#continuous-delivery)
>>> - [Understand and demonstrate continuous integration](#continuous-integration)
>>> - [Understand and demonstrate feature decoupling](#feature-decoupling)
>>>
>>> ### Platforms, Services, and Solutions
>>> - [Understand and demonstrate knowledge of cloud computing fundamentals, including the various tools, services and principles](#cloud-computing-fundamentals)
>>> - [Understand and demonstrate the design patterns and practices for building cloud native services](#design-patterns-and-practices-for-building-cloud-native-services)
>>> - [Understand and demonstrate the use of logging and monitoring tools and infrastructure](#logging-and-monitoring)
>>> - [Understand the need for mobility and migration of data from on-premise to cloud solutions, and the implications](#migration-of-data-from-on-premise-to-cloud-solutions-and-the-implications)
>>> - [Understand the relationship between scaling techniques, how to exploit them, and where](#relationship-between-scaling-techniques)
>>> - [Understand the topologies of enterprise solutions, and how clients use our portfolio of products and services together](#the-topologies-of-enterprise-solutions-and-how-clients-use-our-portfolio-of-products-and-services-together)
>>> - [Understand the various platforms, their differences, relative strengths / weaknesses, and integrations](#the-various-cloud-platforms)
>>>
>>> ### Principles and Practices
>>> - [Demonstrate ability to analyze data sets, identify insights, and leverage to drive decision making](#gather-data-and-conduct-analysis-to-draw-insights)
>>> - [Demonstrate key teamwork and collaborative behaviors](#teamwork-and-collaboration)
>>> - [Demonstrate strong communication skills](#communication)
>>> - [Understand and demonstrate social coding behaviors](#social-coding-behaviors-and-shared-github-repository)
>>> - [Understand and model good feedback behaviors](#giving-and-receiving-feedback)
>>> - [Understand, articulate, and demonstrate agile principles and practices](#agile-principles-and-practices)
>>> - [Understand, articulate, and demonstrate IBM Design Thinking](#ibm-design-thinking)
>>>
>>> ### Professional Skills Development
>>> - [Complete Ways of Thinking module in Apprenticeship Skills Launchpad](#ways-of-thinking-module)
>>> - [Complete Ways of Working module in Apprenticeship Skills Launchpad](#ways-of-working-module)
>>>
>>> ### Software Engineering Fundamentals
>>> - [Demonstrate knowledge of key computer programming fundamentals](#key-computer-programming-fundamentals)
>>> - [Understand and demonstrate how to build and test quality code, at scale](#test-and-scale-code-with-enzyme-and-jest)
>>> - [Understand and demonstrate key software design fundamentals](#key-software-design-fundamentals)
>>> - [Understand and demonstrate knowledge of web programming skills](#web-programming-skills)
>>> - [Understand and demonstrate test-driven development](#test-driven-development)
>>> - [Understand and manage technical debt](#technical-debt)
>>> - [Understand and navigate the complexity associated with enterprise-level development](#enterprise-level-development)
>>> - [Understand how to use version control for all elements of the software delivery lifecycle](#version-control-in-the-software-delivery-lifecycle)
>>> - [Understand, articulate, and demonstrate clean coding behaviors](#understand-articulate-and-demonstrate-clean-coding-behaviors)

---

# Gather Data and Conduct Analysis to Draw Insights
Data Analytics is the process of analyzing raw data and converting it into useful information to find trends or answer questions. It's a broad term that encompasses a vast amount of techniques and many different goals.
How do we analyze data?
- Collect the data
	- From surveys, polls, internet data, etc.
	- Can be collected as a continuous stream or as a series of separate events
- Process the data
	- Parse the data
	- Check for empty fields or errors
- Visualize the data
	- Structure it in a way that is easily readable
	- Popular enterprise tools are SAP, Oracle, or Apache Spark
- Make a decision about your findings
	- Typically done by data scientists or data analysts
	- Recently the industry has turned to AI to make decisions

An example of collecting data would be using something such as a web scrapper. It automates the collection of HTML data from websites. Let's say you wanted to gather data about severe weather trends and analyze it. [Weather.com](https://weather.com/) has articles and videos about important weather events listed on their homepage.
- First we would have to write code to gather the data (Node.js)
	- Click [here](https://github.com/Cosiamo/top-weather-stories) to see the full code on GitHub

```javascript
const PORT = 8000
const axios = require('axios')
const cheerio = require('cheerio')
const express = require('express')

const app = express()

const url = 'https://weather.com/'

axios(url)
	.then(response => {
		const html = response.data
		const content = cheerio.load(html)
		const articles = []

		content('.ContentMedia--listItem--xVM3X', html).each(function() {
			const title = content(this).text()
			const url = content(this).attr('href')
			articles.push({
				title,
				url
			})
		})
		console.log(articles)
	}).catch(err => console.log(err))

app.listen(PORT, () => console.log(`Server is running on PORT ${PORT}`))
```

- Once the code is executed it will dump all the info into the terminal

```
Server is running on PORT 8000
[
  {
    title: 'VideoHuman Chain Saves Tourist Caught in Rough Surf',
    url: '/news/weather/video/human-chain-saves-tourist-caught-in-rough-surf'
  },
  {
    title: 'VideoFish Fall From Sky in TX Storm',
    url: '/news/weather/video/texas-storms-bring-free-falling-fish'
  },
  {
    title: 'VideoAnalysis: 40% of Americans Live in County Struck by Climate Disaster',
    url: '/news/climate/video/analysis-40-of-americans-live-in-county-struck-by-climate-disaster'
  },
  {
    title: 'VideoBig Chill Across County',
    url: '/storms/winter/video/big-chill-across-county-but-relief-on-the-way'
  }
]
```

- We now have data, however the format is terrible
	- The URL is just a directory path
	- All titles have "Video" in front of them
- We need to reformat the code so that it will display the data we want
	- Parse the data

```javascript
// From
content('.ContentMedia--listItem--xVM3X', html).each(function() {
			const title = content(this).text()
			const url = content(this).attr('href')

// To
content('.ContentMedia--listItem--xVM3X', html).each(function() {
			const title = content(this).text().replace(/(Video)/, "")
			const url = 'https://weather.com' + content(this).attr('href')
			articles.push({
				title,
				url
			})
		})
		console.log(articles)
	}).catch(err => console.log(err))
```

- Now that the code has cleaned up some of the formatting, it is much easier to analyze

```
Server is running on PORT 8000
[
  {
    title: 'Human Chain Saves Tourist Caught in Rough Surf',
    url: 'https://weather.com/news/weather/video/human-chain-saves-tourist-caught-in-rough-surf'
  },
  {
    title: 'Fish Fall From Sky in TX Storm',
    url: 'https://weather.com/news/weather/video/texas-storms-bring-free-falling-fish'
  },
  {
    title: 'Analysis: 40% of Americans Live in County Struck by Climate Disaster',
    url: 'https://weather.com/news/climate/video/analysis-40-of-americans-live-in-county-struck-by-climate-disaster'
  },
  {
    title: 'Big Chill Across County',
    url: 'https://weather.com/storms/winter/video/big-chill-across-county-but-relief-on-the-way'
  }
]
```

There are plenty of options when choosing how to visualize this data. You could send it to a MongoDB server, an Excel spreadsheet, or just a simple .csv file. Those are only a few simple examples of what you can do to visualize the data. All you need to do is, instead of calling `articles` as the argument in `console,log()`, extract the `articles` array to any visualization tool of your choice. Once it's visualized, data scientists and/or analysts can make a decisions based upon the findings.

In this example there's only four pieces of information. In a real world case there would be thousands, if not millions of data points. If we wanted to we could then parse information from each URL, but that would make this example much longer than it needs to be.

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

# Work Process

While completing a course on React JS I performed continuous integration, continuous delivery, and contributed to an open GitHub repository I created. Click the link [here](https://github.com/Cosiamo/expense.manager-learning.purpose.only) to see the work.

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

One benefit that feature decoupling offers is the ability to segment your user base. This allows you to decouple specific features between certain users, administrators, and developers. By doing this you're not only doing feature decoupling for functionality and security, but by making the program more dynamic from user-to-user as well.

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
```html
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

# Cloud Computing Fundamentals

There are many tools and options in regards to cloud computing. There are Virtual Private Servers (VPS) which are private servers running on a virtual machine (VM) in the cloud. There's also cloud storage solutions such as Google Drive, iCloud, and OneDrive. Those are just a couple general examples. To get a better understanding of cloud computing, we need to understand the different types of services they provide.

### Cloud Computing Services

#### Infrastructure as a Service (IaaS)

- Infrastructure that is delivered for outsourcing in order to support an organization's operations
- Includes hardware, software, data centers, servers, and network space
- Sometimes referred to as Hardware as a Service (HaaS)
- Implements platform virtualization

#### Platform as a Service (PaaS)

- Web service integration
- For application development, PaaS provides:
	- Hardware and software tools
	- Database management systems
	- Programming language libraries
	- Editing, compilation, and testing tools
	- Version control management

#### Software as a Service (SaaS)

- Software distributed is centrally hosted and licensed
- Similar to Application Service Provider (ASP)
- Single copy of the application is created and given to all users
- Easy API integration

#### Function as a Service (FaaS)

- Run, build, and secure an application with all of it's functionality and services
- Event-driven and users are charged based on their usage
- Tasks can be easily scheduled
- Users don't need to worry about the server
	- Or their inner workings

#### Network as a Service (NaaS)

- For organizations or individuals that that do not want to use their own network
- Network infrastructure hosted by a service provider
- Represents the network as transport connectivity
- Network Virtualization

### Cloud Computing Deployment

#### Public Cloud
- Information is available for everyone who uses the cloud

#### Private Cloud
- Infrastructure is located inside an organization and is not shared with anyone without permission
- Hardware and software are built only for the owner

#### Hybrid Cloud
- Combination of private and public cloud deployments
- Advantage of public cloud while doing heavy workloads and the information is secure as private cloud is incorporated

#### Community Cloud
- Deployment model provided for a limited number of individuals and organizations so that the services are enjoyed only within themselves

### Cloud Computing Vendors

The three biggest service providers in the cloud market are:
- AWS
- Google Cloud
- Microsoft Azure

But, they're not the only ones. A few other big names are:
- Linode
- Heroku
- Alibaba Cloud
- Oracle
- IBM Cloud
- Salesforce
- SAP
- Digital Ocean
- VMWare
- Firebase
- Kamatera
- Vultr
- UpCloud
- Brightbox
- Netlify

---

# Design Patterns and Practices for Building Cloud Native Services

When it comes to designing cloud native applications, there are so many things to consider. Different enterprises will have their own opinions about what the best practices are. In general. most follow these basic practices:
- Proactive planning
- Security
- Reliability
- Performance Efficiency
- Cost Optimization
- Operational Excellence
- Multi-Cloud Approach

#### AWS Cloud Design Patterns
AWS has their own basic design patterns that they recommend:
-   **Snapshot Pattern**(Data Backups)
	-   Ensures data is safe
-   **Stamp Pattern**(Server Replication)
	-   Reduces the time, labor, and expense of setting up a virtual server
-   **Scale Up Pattern**(Dynamic Server Spec Up/Down)
	-   Allows the builders to adjust the server specifications for usage
-   **Scale Out Pattern**(Dynamically Increasing the Number of Servers)
	-   Match unexpected variations in traffic volume
-   **On-Demand Disk Pattern**(Dynamically Increasing/Decreasing Disk Capacity)
	-   Installation of virtual disks provides unlimited capacity while cutting up-front costs

Other types of AWS patterns:
-   High Availability
-   Processing Dynamic Content
-   Processing Static Content
-   Uploading Data
-   Relational Database
-   Batch Processing
-   Operation and Maintenance
-   Network

#### Microsoft Best Practices in Cloud Applications
Microsoft, just like most major cloud vendors, has their own best practices recommendations when building a cloud native application. These include:
- **API design**
	- Most modern web applications expose APIs that clients can use to interact with the application
	- A well-designed web API should support platform independence and service evolution
- **API implementation**
	- A carefully designed web API defines the resources, relationships, and navigation schemes that are accessible to client applications
- **Autoscaling**
	- The process of dynamically allocating resources to match performance requirements
- **Background jobs**
	- Can be executed without requiring user interaction
	- The application can start the job and then continue to process interactive requests from users
- **Caching**
	- Temporarily copying frequently accessed data to fast storage that's located close to the application
- **Content delivery network (CDN)**
	- Distributed network of servers that can efficiently deliver web content to users
- **Data partitioning**
	- Data is divided into partitions that can be managed and accessed separately
	- Partitioning can improve scalability, reduce contention, and optimize performance
- **Data partitioning strategies (by service)**
	- There's a listed strategy for partitioning: SQL Database, Azure table storage, Blob Storage, Azure storage queues, Service Bus, Cosmos DB, Azure search, Cache for Redis, Service Fabric, and Event Hubs
- **Message encoding considerations**
	- An important aspect of messaging is the format used to encode the payload data
	- A few choices for encoding formats are: JSON, CSV, protobuf, Apache Avro, MessagePack, and CBOR
- **Monitoring and diagnostics**
	- Track the way in which users use your system
	- Trace resource utilization
	- Monitor the health and performance of your system
- **Transient fault handling**
	- The momentary loss of network connectivity to components and services
	- The temporary unavailability of a service
	- Timeouts that arise when a service is busy

---

# Logging and Monitoring
Logging the activity of your application is vital in understanding how your users interact with the application as well as monitoring the status of the application itself. There are a few best practices to remember in order to have a good log.

## Four dimensions of a good log
- Context
- Purpose
	- Debugging
	- UX
- Importance
	- Is it urgent? Did the users credit card info get leaked?
	- Is it normal activity? Did a user log in?
- Format
	- Scannable
	- Parsable

Understanding and properly recognizing what type of log activity is going on will help you determine the appropriate response. That's why properly formatting the log as well as showing ample context is extremely important.

**Bad Format**
```
user 32 logged in at 1522775572388
user 68 logged in at 1522775580450
fatal error: [object Object]
got some data [ [object Object], [object Object],
[object Object], [object Object], [object Object],
[object Object], [object Object], [object Object],
...]
```

- Object , object, object takes hundreds of lines of logs
	- You lose the context
- Can't expect anyone reading to know UNIX Epoch off the top of their heads

**Good Format**
```
[Tue, 03 Apr 2018 17:16:10 GMT] user [32] logged in
[Tue, 03 Apr 2018 17:16:12 GMT] user [68] logged in
[Tue, 03 Apr 2018 17:16:16 GMT] Failed to load user data: [
	[MYSQL DB] Too Many Connections
	... Stack Trace
]
[Tue, 03 Apr 2018 17:16:18 GMT] Server recieved [46] timestamps from [82.536.22.367]
```

- Readable error message
- Wrapping things in square brackets makes things really parsable
- Having human readable timestamps is much more useful

**Bad Context**

```
a user logged in
a user logged in
a user logged in
a user logged in
a user logged in
error
got data
```

- Doesn't tell you if they're unique users or the same one
- No context for the error is
- Doesn't tell you what the data is

**Good Context**

```
[Tue, 03 Apr 2018 17:16:10 GMT] user [28] logged in
[Tue, 03 Apr 2018 17:16:10 GMT] user [28] logged in
[Tue, 03 Apr 2018 17:16:10 GMT] user [28] logged in
[Tue, 03 Apr 2018 17:16:10 GMT] user [28] logged in
[Tue, 03 Apr 2018 17:16:10 GMT] user [28] logged in
[Tue, 03 Apr 2018 17:16:16 GMT] Failed to load user data: [
	[MYSQL DB] Too Many Connections
	... Stack Trace
]
[Tue, 03 Apr 2018 17:16:18 GMT] Server recieved [46] timestamps from [82.536.22.367]
```

- Shows the same user logged in multiple times at the exact same time
- Either a bug in the code or the user is a bot
- The error shows that there are too many connections
- The data that was received shows that a specific IP address was pinging the application 46 times

## Logging Golden Rules
There's more to creating a good log than writing out the log itself. You also have to make sure the information inside the log follows best practices as well. An easy way to figure this out is to follow the three **Logging Golden Rules**:
- Remember the reader
- No Personal Data
- Answer the Question!

## The Logging Type Table
To expand more on the **Logging Golden Rules** it helps to understand **The Logging Type Table**.

| Name | Goal | Question | Content | Audience |
| :----: | :----: | :----: | :----: | :----: |
|*Tracing* |Debugging |How'd we get here? |State changes |Devs |
|*Event* |Analytics |What's happening? |Actions & Exceptions |Admins / Product Owners / Devs |
|*Progress* |UX |RUOK? |Percentages / Errors |Users |
|*Audit* |Verification |RU lying? |Summaries, Double-entry book keeping |Compliance, Gov, Business |



*Trace logging*
- Often used for debugging
- Logging functions or individual variables
- When there's some bug that you're looking at
- Useful for developers

*Event logging*
- Analytics
- Understanding of what's going on inside the app
- Is it the user or application doing specific things?
- Know which bits of specific code to improve
- Useful for administrators, product owners, and developers

*Progress logging*
- Useful feedback to users of applications
- Can show the percentage of progress during installation
- All about answering the question "Are you ok?"
	- "Are you still working?"
	- "Have you crashed?"
	- "Are there any errors?"
-  Lets the user know what's going on

*Audit logging*
- Verifying that the behavior of the application is correct
- How many users are using the tool?
- Is the number of subscribers going down every month?
- It's essentially Double-entry Book Keeping
	- What accountants do
	- Look at the data from different angles
- This information is very useful for the compliance team, government regulators, and businesses

## Common Tools
- Log4j
- Syslog standard
- Count.ly
- Logstash
- console.*
	- log
	- error
	- warn
	- info
	- timing
	- profiler


---

# Migration of Data from On-Premise to Cloud Solutions and the Implications

### Deployment
**On-Prem:**
- The entire infrastructure is located on your property
- Everything is controlled by the IT team on staff
	- They're responsible maintaining and upgrading everything themselves

**Cloud:**
- The infrastructure is on the premises of whichever company you purchased your cloud subscription from
- you can access all the resources you use and use as much as you want at anytime

### Cost
**On-Prem:**
- Purchase all server hardware
- Pay the electricity bill
- Buy or rent enough land and building space to store it in

**Cloud:**
- Only pay for resources you use
- No maintenance and upkeep cost
- Price is determined by how much is used

### Control
**On-Prem:**
- You retain and control all of your data
- Better for highly sensitive data

**Cloud:**
- Data resides on a third-party server
- Risk of downtime out of your control
- Third-party can hold some of your encryption keys

### Security
**On-Prem:**
- Organizations with sensitive information should use an on-premise server
	- Hospitals
	- Banks
	- Government Organizations

**Cloud:**
- Security threats are much more serious
- Many high profile businesses and organizations have had highly publicized security breaches
- It's the one big drawback to cloud and why many organizations choose to keep private data on their own in-house servers

---

# Relationship Between Scaling Techniques

## AutoScaling
- A feature that allows organizations to scale their cloud services automatically
- Allows organizations to scale up or down based upon external traffic or internal utilization
- No need for manual intervention during spikes in traffic

**Advantages**
- Lower cost
- Reliable security
- Consistent availability

**Types of autoscaling**
-   Predictive
    - Anticipates known changes in traffic
    - Executes reactions at specific times
-   Scheduled
    - Works best when there are known traffic increases or decreases
    - Keeps autoscaling groups on notice in case of spike in traffic
-   Horizontal
    - Adding more servers to the auto scaling group in order to scale
-   Vertical
    - Scaling by adding more power rather than more servers
    - Additional RAM, CPU, or storage

## Fail Fast
- Process of starting a project, gathering feedback, then determining if you should continue the current work, take an alternative approach, or abandon the project
- This method allows teams to experiment with new ideas more often

**Advantages**
- Quickly identify bugs
- Makes the team  working on the project more efficient
- Reduces the amount of wasted time, energy, and cost

## ReplicaSets
- ReplicaSets are Kubernetes controllers that are meant to manage a set of replica pods and keep them running at all times
	- Will add or delete pods based upon what's needed
- A pod must be labeled with a matching label to the ReplicaSet selector
	- Pods can be isolated from a ReplicaSet by changing their labels so that they no longer match the ReplicaSet???s selector
- Can replicate pods and restart or spin up new pods when ones that already exist fail

## Scale up
- Scaling up is when you purchase more powerful hardware to scale your project
	- CPU
	- RAM
	- Storage
	- Faster network speeds
	- etc...
- The goal is to increase the performance of the hardware that supports your application
	- A more powerful single server
- If your using a cloud solution it can be as easy a few button clicks
- If your using an on-prem solution it could take days or weeks for the hardware to come in, install it, and to have ready for deployment
- A way to think of scale up:
	- Buying a *larger* refrigerator when you start to run out of room for food

## Scale out
- Scaling out is when you use software to scale to more than one server
	- Each server is considered a node
- One way to scale out is by creating a new container running a web-server app and adding it to the load balancer pool
- Projects that support this type of scaling also support the ability to remove resources when the load decreases
- A way to think of scale out:
	- Buying *more* refrigerators when you start to run out of room for food

---

# The Topologies of Enterprise Solutions and How Clients Use Our Portfolio of Products and Services Together

IBM has been an industry leader for over a century, and as a result, we offer many products and solutions. However, IBM adapts it's business model to best serve it's clients with the newest technologies and innovations. The past few years there has been a strong focus on **Hybrid Cloud**, **Artificial Intelligence** (AI), and **Quantum Computing**. IBM has dedicated so much to these technologies that we recently split our other legacy IT services to our new sister company, Kyndryl.

## Hybrid Cloud
- *Hybrid Cloud* is a combination of public cloud, private cloud, and on-premise infrastructure that are seamlessly integrated
	- Provides better management and greater application portability
- It is a single, unified, and flexible environment while each type of cloud model remains as it's own separate entity
- Allows organizations to run and scale traditional or cloud-native services on whichever model suits their specific needs

**Advantages**
- Improved developer productivity
	- Helps development teams adopt Agile and DevOps methodologies
	- Develop once and deploy to all clouds
- Greater infrastructure efficiency
	- Allows development teams to have greater control over their resources, development, and IT operations
	- Optimize synchronization across public cloud, private cloud, and cloud vendors
	- Teams can modernize applications faster
	- Teams can connect cloud services to data on cloud or on-premise servers to drive greater efficiency
- Improved regulatory compliance and security
	- Organizations can keep sensitive information on-premise while having the benefits of scalability that cloud services offer
	- Allows the ability to implement regulatory compliance across all environments
- Overall business acceleration
	- The efficiency of the hybrid cloud model enables shorter software development life cycles
	- Accelerates the time to implement new features and time-to-market
	- Faster third party integration

## Artificial Intelligence
- *Artificial Intelligence*, by definition, is intelligence demonstrated by machines
	- Any system that can perceive it's environment and take the necessary actions to achieve it's goals
- Alan Turing, who is considered the father of computer science and artificial intelligence, proposed an experiment referred to as the Turing Test
	- The idea is that if a machine is truly intelligent, a human interrogator wouldn't be able to tell the difference between a human's response and a computer's response
	- Although this test would later be scrutinized, it is still considered an important part of the history of AI research as it helped evolve how humans think about our relationship with computers
- IBM has been a leader in AI innovation for enterprises
	- IBM Watson offers AI tools to enterprises that significantly improve their workflows, enhance automation, and increase efficiency
- One key concept that has enabled IBM to be at the cutting edge of AI implementation is the AI Ladder

**AI Ladder**
- Artificial Intelligence
- Machine Learning
- Analytics
- Data and Information Architecture

**Services IBM Watson Delivers on for AI Needs**
- Advertising
- Business automation
- Customer service
- Explainable AI
- Financial operations
- Healthcare
- IT operations
- Natural language processing
- Return to work
- Risk and compliance
- Security
- Supply chain
- Video
- and much more...

## Quantum Computing
- *Quantum Computing* is a type of computation that utilizes quantum physics to perform advanced calculations and difficult computations
- They do this by using the properties of quantum state, such as:
	- Superposition
	- Interference
	- Entanglement
- Computers, at the base level, are able to execute commands from the user by reading binary
- Traditional CPUs read the state as either 0 *or* 1
	- Known as bits
- Quantum CPUs are able to be 0 *and* 1 at the same time
	- These are called qubits
- IBM introduced the first commercial quantum computer, the IBM Q System One, in 2019
	- At the time it was only 20 qubits
	- The new Eagle processor will have 127 qubits

**IBM Quantum Services**
- IBM Quantum Services allows users to run circuits 120 times faster
- Over 20 systems available
	- Hardware, systems, simulators, and runtimes
- Access to APIs and programming tools

---

# The Various Cloud Platforms

The big three in the cloud computing business are:
- **Amazon Web Services**
- **Microsoft Azure**
- **Google Cloud**

The first of the three is AWS, which was released in July 2002. Later Google released Google Cloud in April 2008, then Microsoft followed shortly after, releasing Azure in October 2008. AWS is the largest and has the advantage of pioneering the industry. Azure is great for customers who regularly use Microsoft tools as it allows easy integration. Google Cloud has the best pricing model as well as easy integration for Google products and tools. That's just a basic overview, the big differences are much more in depth. The website [javatpoint](https://www.javatpoint.com/) has a great table showing all of the key differences between these services.

Parameter | AWS | Azure | Google Cloud
:----:    |:---:|:---:  |:----:
  **App Testing**| device farm | DevTest labs |Cloud Test labs
 **API Management** | Amazon API gateway | Azure API gateway |Cloud endpoints
 **Kubernetes Management** | EKS | Kubernetes service |Kubernetes engine
  **Git Repositories**| AWS source repositories | Azure source repositories |Cloud source repositories
 **Data warehouse** | Redshift | SQL warehouse | Big Query
  **Object Storage**| S3 | Block Blobs and files |Google cloud storage
  **Relational DB**| RDS | Relational DBs |Google Cloud SQL
  **Block Storage**| EBS | Page Blobs |Persistent disks
  **Marketplace**| AWS | Azure |G suite
  **File Storage**| EFS | Azure Files |ZFS and Avere
  **Media Services**| Amazon Elastic transcoder | Azure media services |Cloud video intelligence API
 **Virtual network** | VPC | VNet |Subnet
  **Pricing**| Per hour | Per minute |Per minute
  **Maximum processors in VM**| 128 | 128 |96
  **Maximum memory in VM (GiB)**| 3904 | 3800 |1433
  **Catching**| ElasticCache | RedisCache |CloudCDN
  **Load Balancing Configuration**| Elastic Load Balancing | Load Balancer Application Gateway |Cloud Load Balancing
  **Global Content Delivery Networks**| CloudFront| Content Delivery Network|Cloud Interconnect


### Amazon Web Services (AWS)

**Pros**
- Massive community of developers
- An extensive amount of offerings
- Support for large enterprises
- Large amounts of documentation
- Lots of training available
- Servers across the world

**Cons**
- Can be difficult to use
- An overwhelming amount of options
- There have been complaints in regards to pricing

### Microsoft Azure

**Pros**
- Easy Microsoft tools and software integration
- Great hybrid cloud support
- Broad feature set
- Free and Open Source support

**Cons**
- Problems with the documentation
- Management tools are lackluster

### Google Cloud

**Pros**
- Made with cloud-native businesses in mind
- Great discounts happen regularly
- Flexible pricing
- Support for portability
- Free and Open Source support

**Cons**
- Fewer features
- Not as many services as the other two

---

# Social Coding Behaviors and Shared GitHub Repository
I started working on a web scrapper that grabs the top four stories from weather.com and turns the titles and links into objects that are placed inside of an array. I made a  simple `console.log()` call to log the array to the terminal. I wanted to get some help for the front-end so I asked Johnnie Gonzalez on Slack if he could help me with it. We ended up calling each other on Webex, and before he started on the front-end, he gave me some very helpful feedback on the back-end code I wrote.

To view the entire repository click [here](https://github.com/Cosiamo/top-weather-stories).

- Originally I had 2 `url` variables
	- One that contained the website
	- The other inside of the callback function (outside of the global scope)
- I typed out `weather.com` twice instead of using the variable at both locations that I needed it at
	- I couldn't call the original `url` variable because it was being overwritten by the new one inside of the callback function
- Whenever he informed me that I should have one variable for the url, it made me realize that I also statically typed the class name as an argument inside of the `content` function
- He informed me that I needed comments in order for him to have a better understanding of how it works

**Before**
```javascript
const url = 'https://weather.com'

axios(url)
	.then(response => {
	const html = response.data
	const content = cheerio.load(html)
	const articles = []

	content('.ContentMedia--listItem--xVM3X', html).each(function() {
		const title = content(this).text().replace(/(Video)/, "")
		const url = 'https://weather.com' + content(this).attr('href')
		articles.push({
			title,
			url
		})
	})
	console.log(articles)
}).catch(err => console.log(err))
```

- Changed the `url` variable inside of the `content` function to `link`
- was able to call original `url` variable inside `link`
- Moved the class name from the argument in the `content` function to it's own variable
- Added comments
- Also added a README file

**After**
```javascript
// Website you want gather info from
const url = 'https://weather.com'
// The Class or ID name of the element you're targeting
const htmlElement = '.ContentMedia--listItem--xVM3X'

// Axios calls the http address from the url variable
axios(url)
	.then(response => {
	// html variable records the data from the http response
	const html = response.data
	// Cheerio picks out the HTML elements from the html variable
	const content = cheerio.load(html)
	const articles = []

	// function after '.each' is a callback function
	content(htmlElement, html).each(function() {
		const title = content(this).text().replace(/(Video)/, "")
		const link = url + content(this).attr('href')
		// fills the empty articles array
		articles.push({
			title,
			link
		})
	})
	// calls the array and prints to the terminal
	console.log(articles)
}).catch(err => console.log(err))
```

Because we were coding together we taught each other a few things. I thought him about some useful Node.js modules as well as the basic concepts of building a web scrapper. He taught me how to structure my back-end code in a way that is helpful for front-end developers.

After the call, Johnnie pulled the repository from my GitHub and added the front-end. He also changed the back-end some to fit what he was working on.

**His back-end additions**
```javascript
const fetchNestedVideoSrc = async (link, number) => {
	const browser = await pup.launch();

	const page = await browser.newPage();
	await page.goto(link);

	// takes a screen shot of the current page and saves it to make sure we are viewing the correct page we want to scrape
	// await page.screenshot({ path: `story${number}.png` });
	const pageData = await page.evaluate(() => {
		return {
			html: document.documentElement.innerHTML,
	};
});

	const cl = ".jw-video jw-reset ";

	const $ = cheerio.load(pageData.html);
	console.log("PAGE!!!", $(cl, pageData.html).find("src"));

	// closes the browser to use the next link that's retrieved
	await browser.close();
};
```

```javascript
app.get("/express_backend", (request, response) => {
	// sending the articles to the frontend using an app get method
	response.send(JSON.stringify(articles)); //turning the data into a string
});
```

**The directories and files he added for the front-end**

> client
>-  public
>    - favicon.ico
>    - index.html
>    - logo192.png
>    - logo512.png
>    - manifest.json
>    - robots.txt
>- src
>    - components
>        - TopWeatherStories.jsx
>    - App.css
>    - App.js
>    - App.test.js
>    - index.css
>    - index.js
>    - logo.svg
>    - reportWebVitals.js
>    - setUpTests.js

The code I wrote originally only logged the titles and links to the articles in the terminal, but thanks to Johnnie, those are now on a web page.

---

# Agile Principles and Practices
In the 1980s and 1990s, most software failed to be useful to the average person. A lot of it was built over budget and missed deadlines quite frequently. The fast growing market in the early days of the personal computer came with demands and expectations that far outweighed the capacity that software development teams had to build software as it was needed. Many analysts in the industry conducted experiments to find the cause of this "climate of failure" and found that the waterfall development model was to blame.

**Waterfall Model**
- Projects were broken down into linear sequential phases
	- Referred to as a linear-sequential life cycle method
- Each phase depends on the what is delivered from the previous phase
	1. Requirement analysis
	2. System design
	3. Implementation
	4. Testing
	5. Deployment
	6. Maintenance

Because the software development life cycle of the Waterfall Model progressed in one direction, it was not very flexible. As a result of these shortcomings, 17 software developers came together in 2001 to create **The Agile Manifesto**, which lays down the foundations of Agile Principles and Practices.

## The Agile Manifesto
The 17 authors of The Agile Manifesto couldn't agree on everything, however, they did agree on four core values.

**Four Core Values**
- *Individuals and interactions* over processes and tools
- *Working software* over comprehensive documentation
- *Customer collaboration* over contract negotiation
- *Responding to change* over following a plan

According to The Agile Manifesto, there are 12 principles that need to be followed in order for the Agile methodology to work.

**The 12 Agile Principles**
1. Satisfy the customers through early and continuous delivery of valuable software
2. Welcome changing requirements, even late in development
	- Agile processes harness change for the customer???s competitive advantage
3. Deliver working software frequently, from a couple of weeks to a couple of months, with a preference to the shorter timescale
4. Business people and developers must work together daily throughout the project
5. Build projects around motivated individuals
	- Give them the environment and support they need, and trust them to get the job done
6. The most efficient and effective method of conveying information to and within a development team is face-to-face conversation
7. Working software is the primary measure of progress
8. Agile processes promote sustainable development
	- The sponsors, developers, and users should be able to maintain a constant pace indefinitely
9. Continuous attention to technical excellence and good design enhances agility
10. Simplicity
	- Maximizing the amount of work not done is essential
1. The best architectures, requirements, and designs emerge from self-organizing teams
2. At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly

## Agile Practices
Agile Practices take form in the Agile Development Model. This model takes all of the Agile Principles and breaks it down into multiple types of development frameworks. A few examples of these are:

**Scrum**
- An administration framework for project development
- The system is developed incrementally in sprints
	- Usually anywhere from 2 to 4 weeks
- Groups are cross-functional and self-organizing
- Use visual graphs to track the improvements of the project

**Extreme Programming**
- An engineering technique that includes practices that will assure a high quality product
- Teams work in iterations which are typically a couple weeks long
- Four ideals:
	- Communication
	- Feedback
	- Simplicity
	- Courage
- Solely focusing on developing programs and never visualizing improvements with visual aids

**Feature Driven Development**
- Organizes software development around making progress on features
- Typical project life cycle:
	- Develop a general model
	- Make a list of features
	- Plan for each feature
	- Design each feature
	- Build the feature

---

# IBM Design Thinking
IBM Design Thinking is a human-centered, scalable framework designed to help teams understand their customers and continuously deliver products. The core concept is to prioritize users over anything else. It relies on using empathy while observing how people interact with their environments. Instead of designing a a project around it's structural components or technical requirements, it focuses on how to help a user achieve a specific goal.

According to IBM's documentation, these are the foundational elements of design thinking:

## The Principles
**Focus on user outcomes**
- Measure success based on how well we fulfill human needs
- Users are more than just customers, but design partners as well

**Restless reinvention**
- Everything is a prototype
- Continuous conversation with users and clients
- Respond to changing needs through rapid prototyping and constant iteration

**Diverse empowered teams**
- Diversity is the bedrock of a dynamic team
- By empowering people with different backgrounds, perspectives, and skillsets to bring their unique points of view to the table, it enables everyone to generate more breakthrough ideas faster

## The Loop
**Observe**
- To create meaningful outcomes for the users, we need to understand what challenges thy face in their daily lives
- By immersing ourselves in the worlds of our users, we ensure that our solutions always address real-world needs

**Reflect**
- Different people can interpret the same situation in very different ways
- Having those different perspectives come together to reflect on observations helps synthesize and analyze findings, building more nuanced understanding of our users across the team

**Make**
- The only way to see an outcome is to make one
- Rapid, low-fidelity prototyping allows us to simulate ideas and test hypotheses quickly and cheaply
- This creates solutions that are robust, effective, and battle-tested

## The Keys
**Hills**
- Concise statements of goals we aim to help our users accomplish
- By making these goals explicit and evaluating them regularly, we align teams around a single shared mission

**Playbacks**
- Regular check-ins that bring users, stakeholders, and teams together to exchange feedback
- Allow us to to measure progress on a regular basis
- Uncovers and addresses any misalignment that may exist

**Sponsor Users**
- Sponsor users are real-world users that provide teams with deep expertise and knowledge on problems they're facing
- They're crucial to keeping us aligned with our users' reality throughout the course of a project

---

# Ways of Thinking module
> ## Business Acumen
> On the week of Fourth of July I took the ???Core Capabilities Fundamentals??? class in which we learned the best practices in sales meetings. The class was divided into teams and we were tasked with creating a pitch for a fictional telecommunication merger for the end of the week. We got together on WebEx for the next few days creating a Power Point of our proposal, practicing our presentations, and building team chemistry. We used business acumen to create realistic scenarios for what work would have to entail as well as what the time table would look like in order for the project to be completed. At the end of the week our team ended up winning the competition.
> ## Creative Thinking and Innovation
> The group of other apprentices I talk with everyday and I need experience contributing to a group coding project for the apprentice data base and since we haven???t been told when we???re shadowing any real projects we decided to do a group project on our own. We are actively brainstorming what we want to do for the project but we???ve narrowed it down to making the periodic table of elements with a front end showing the table and a SQL database that lists the properties of each element. The reason we???ve decided this particular project is because we need to complete the specific competencies ???Demonstrate key teamwork and collaborative behaviors??? and ???Understand and demonstrate social coding behaviors???. Because of this we are restricted to certain criteria that we need to complete. Those are: demonstrate successful use of pair programming, demonstrate successful use of playbacks and reviews, demonstrate knowledge of social coding behaviors, execute successful ???Fork and Pull??? model in GitHub, understand the use of Open Source and the implications, and a few more that are similar. Those restrictions are helping us in our creativity of what we plan on accomplishing through this project. That is how we came to the result of creating the periodic table with those criteria in mind in every step of the process.
> ## Critical Thinking and Problem Solving
> One situation where I applied critical thinking skills in the workplace was when I was working for AT&T and I had to find a way to boost business sales for our store in a very competitive market. I first asked myself, ???why are business sales down???? And the answer was because this was the fourth AT&T store they opened in a 10 mile radius. Then I asked ???why don???t we find a way to differentiate ourselves from the stores???? So the answer was clear and simple, find a way to make us stand out. The follow up question to that was ???what are some ways we can do that???? Well, if the customers weren???t coming to us, we should go to the customers. ???Where can we meet the customers???? We decided that we should go to businesses??? locations and events. And finally, ???why would they choose us to help them???? After contemplation we decided that the best way to do that was to have un-rivaled customer support. Be there for their every question they have and help them with device transfers, smartwatch setups, and Bluetooth setups. The result was that we boosted our store???s business sales by over 130% from the prior month.
> ## Decision Making
> When I was working in mobile sales, I would frequently encounter fraud attempts. What that means is that a customer would use a fake ID, use someone else???s account, or have another person that doesn't care about their credit score to take out a lease on a phone they don???t plan on paying. One example of this situation is, when I was working as a third-party representative for AT&T at Best Buy, we had an older man claim he was opening a new account to get two brand new iPhones for his daughters. We had different ways of identifying potential fraud and he was checking a few the boxes (ID had red font on the top right saying it wasn't for federal use and he was just asking about the initial down payment and didn???t ask anything about the monthly costs). However, you can???t flag someone as fraud without obtaining more information because you could be losing a legitimate customer if you???re wrong. So, the task at hand was to decide if I should flag the account or proceed with the sale. The actions I took were asking some questions I???ve come up with to narrow down if it???s legitimate, such as: ???Do you need help transferring data or setting up the devices????, ???Do you want to know about the features and prices between the different plans we offer????, and ???Are you sure you don???t want to add your number to this new account? It???ll be the third line which gives you a multi-line discount that???s cheaper and has better features than your current plan.???. Those questions sound like basic questions you would hear at a cell phone store and that???s the point, you don???t want to offend the customer with blunt questions if they are legitimate and you gain valuable information into proceeding with the sale. The reasons you would use those questions in particular is because people committing fraud don???t want you setting up the phone(s) because they plan on selling them as unopened/brand new, they don???t care about the features or prices of the plans because they never plan on paying or using the lines, and they don???t want to move their personal number on the account even if it saves them money and/or offers better features because they know it???ll link them personally to the fraudulent account. This customer???s answers to the questions raised more concerns with me because it was seeming more likely that this was fraud. The result of this interaction made me go tell the mobile sales supervisor all the information I???ve gathered and she deemed it as fraudulent so we flagged the account and came up with a polite way of telling him that the carrier won???t let us proceed with the transaction and that he would have to contact AT&T directly. If the sale had gone through our store would have had to absorb the cost of two iPhone 12 Pro Maxes worth $1449.99 each.

---

# Ways of Working module
> ## Adaptability and Flexibility
> One time I had to adapt and be flexible in the workplace was when I was working at Boost Mobile and I was promoted to lead repair tech of a different store location and I had no idea about it until I clocked in that day. The task at hand was to set up a repair shop in the back of the store that never had one. In order for me to succeeded at this task I needed to start from the ground up and communicate daily with the franchise owner. I had to get a table, new tools from iFixit, and get parts from our supplier. I adapted to the new store quickly and started repairs almost immediately. I also was able to train a few people in the process.
> ## Being a Trusted Advisor
> During my time in mobile phone sales I had to be a trusted advisor in order to successful at my job. When I ended up at Best Buy as an AT&T rep. I had the opportunity to be a leader, which meant that I had to know the entire sales process from the inside-out. As a result of knowing all of the deals and promotions, I built many strong client relationships throughout my tenure because I was always able to get my customers the best deals. I was not only a trusted advisor to my clients, but to my coworkers as well. They all activated phones and plans for every carrier that Best Buy offered so there was no way they could remember everything. So they relied on me for information in regards to all of the AT&T plans and promotions.
> ## Leadership
> One time I showed leadership in the workplace was when I was working for Boost Mobile. The franchise owner moved our store manager to another location without appointing a new manager at our store, so I stepped up to fill the role. I would manage our store???s inventory, set up hours for my coworkers and I, and reported sales results to our franchise owner and Sprint rep. I even trained a new repair technician. Even though our store???s volume was always low, I still was able to keep it afloat and meet monthly goals while I was filling the role without any proper manager training whatsoever.
> ## Teamwork and Collaboration
> I actively practice teamwork and collaboration daily with Johnnie Gonzalez, Matthew Crain, and Skyler Pointer. Every day we talk with each other on Slack to see where we are in the apprentice launchpad and apprentice database. We answer each other???s questions and help with problems that any of us have. We set up WebEx meetings every so often so we can collaborate and plan our next steps in order to complete the apprenticeship as soon as possible. The result has been that we???ve completed a large chunk of what we???ve been assigned and gained a plethora of knowledge regarding various coding languages, proper coding syntax, and a better understanding of SAP and all of the intricacies in their partnership with IBM.
>## Giving and Receiving Feedback
>When giving feedback it's best to share in a respectful way that will help the person you're communicating with understand your thoughts so they can take the necessary steps to improve their performance. Make sure to focus within the context of the specific goal you set out to achieve. You want to help them improve, not bring them down with unnecessary criticism. Timing is key as well. There's no need to bring up your thoughts on an unrelated topic that doesn't have any relevance to the one at hand.
>When receiving feedback make sure to ask for feedback on all of your work as much as you can. In order to better yourself and the work you do, you need to understand your shortcomings on a particular topic. Instead of being defensive and interrupting them in the middle of their thought, it's better to hear them out completely and to take in what they're saying. Not everyone is perfect and some of their points you may disagree with. Don't get angry and try to explain your point immediately, instead ask them questions about how they came to that conclusion. They may bring up a good point you haven't thought of yet, or if they're incorrect, it's easier to explain your point of view on a specific part of their feedback instead of their entire statement as a whole.
>## Communication
>One time I had to use my communication skills at work was when we took the ???Ehub School June 2021??? from June 14th-25th. Our goal was to to create a presentation for two fictional clients that were merging together and at the end of the week we had to present the proposal. During my turn to speak I was nervous but prepared. I wrote down some bullet points on the notepad app and had it positioned below my webcam so it didn't seem like I was breaking eye contact. When the instructors asked me difficult questions I confidently answered them. After all the groups presented, the instructors announced that we won the competition.
>## Cultural Intelligence
>It's very important to have cultural intelligence, not just at work, but in life in general. Understanding that everyone comes from different backgrounds and cultures will help with gaining more friends, finding great business partners, and being more understanding of someone's feelings about particular subjects. Being culturally aware is important because something that might mean nothing to you, could insult someone else. It's imperative to understand other cultures. You can learn more by researching the culture or just by listening to what someone from that culture has to say.
>## Emotional Intelligence
>Having emotional intelligence at work is key to having a better working relationship with your coworkers and clients. When I was working in mobile phone sales I had to practice emotional intelligence everyday. Whether a customer was upset about an unexpected charge on their phone bill, angry about a their phone being broken, or worried about losing their photos during an upgrade. I had to read the room in order to respond appropriately to the situation.
>## Working Social
>Most people have worked in a social environment at some point in their lives. Ever since I started working I've worked socially. Even though I work remotely now I still work in a social setting, although virtually. The team I'm on still communicates daily on Slack and when we need help or just to check where everyone is at we get on a Webex call. I find it very helpful because if I'm ever stuck or have a question, it's great having a coworker that understands what needs to be done and helps you out.
>## Presentation and Storytelling
>Presentation and Storytelling were vital to my sales jobs in the past, especially when I was selling cable at AT&T. In the age of "cord cutting" it's much more difficult to sell cable packages. Why would anyone spend over $200 a month when Netflix is $9? In order to convince the customer that they should get cable I would first ask them about what type of entertainment they enjoy, then I would use that to plan out my presentation. Each store had a DirecTV display, so I would bring my potential client over there and give them a hands on demonstration of how DirecTV worked and what type of content the various channels offered that would be beneficial to them. Honestly it was difficult, unless they enjoyed sports. When I would get a sports fan as a customer my presentation sold them on getting a cable package all the time.

---

# Key Computer Programming Fundamentals

### Functions
- A block of reusable code that was created to perform a task

```javascript
const square = function(x){
	return x * x;
};
```

### Objects
- An abstract data type that can include properties, methods, and other objects

```javascript
const car = {
	brand: "Ford",
	model: "Mustang",
	color: "Red"
};
```

### Methods
- A subroutine from a class that is predefined in the source code of a program
- defines the behavior of a class

### Arrays
- An ordered collection of multiple items of the same type stored together indexed by contiguous integers

```javascript
const pets = [
	"Dog",
	"Cat",
	"Parrot"
];
```

### Variables
- A malleable storage location which contains information (data or code)
- Typically the name of the variable will reference the information it contains

```javascript
let pet1 = "Dog";
let pet2 = "Cat";
let pet3 = "Parrot";

const pets = [
	pet1,
	pet2,
	pet3
];
```

### Loops
- **For Loops**
	- Enables a set of conditions to be executed repeatedly until a condition is satisfied
	- Repeats a unit of code a known number of times

```javascript
for (let i = 0; i < example.length; i++) {
	example[i];
}
```
- **While Loops**
	- Repeats a a set of instructions based on a condition
	- Repeats a unit if code an unknown amount of time

```javascript
while (i < 10) {
	"A number that is less than 10 is " + i;
	i++
}
```

### Errors
-  **Logic Error**
	-  A type of error that is the result of improper logic in code that was written
	-  Can cause unexpected results
	-  might not prevent you from running a program, but might crash it later
-  **Syntax Error**
	-  Caused by writing incorrect syntax according to the language you're programming in
- **Runtime Error**
	-  An error that occurs while the program is running after being successfully compiled
	-  Often referred to as bugs

### Debugging
- The process of detecting and removing bugs, aka, errors

### Data Structures
- A way to store and retrieve data so that a computer can use it effectively

### Recursion
- The process of defining a problem or a solution in simpler terms
- Take one big problem and break it down into smaller problems

### Pseudocode
- A method of creating instructions (algorithms) in a human readable format
- "fake code"

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

# Key Software Design Fundamentals

There's many things to take into consideration when designing software. It's not a good idea to bodge things together, it's better to plan out how to tackle the challenges ahead.
- Focus on Structure
- Anticipate expensive choices
- Make sure core decisions strive for high quality

Using design principles will help you come up with a plan to structure your design.

## Divide Up the Tasks
-   Take one large problem and break it down into as many sub-problems as possible
-   As you solve the smaller problems it will help you solve the larger problem
-   Let's say you're working on a large scale system
	-   That large system might have sub-systems
		-   figure out all of the sub-systems of the system as a whole
	-   Those sub-systems might have different packages
		-   figure out what all of the packages of the sub-system are
	-   Those packages might have different classes
		-   figure out what all of the classes of the packages are
	-   Classes have different methods
		-   figure out what all the methods of the class are
	-   Methods might use some external dependencies or functions  

## Cohesion
-   Everything grouped together makes sense
-   Designing in a way that everything relates to each other so that nothing doesn't fit in
-   Designing packages, modules, and classes so that everything is cohesive
-   Everything is grouped together that should be together
-   An example of this are math packages in a language like Python, JavaScript, or Java
    -   considered very cohesive because everything related to math operations is in that package and nothing unnecessary
        -   sqrt
        -   pow
        -   log
        -   add
        -   etc...

## Avoid Coupling
-   I go over this in the [Feature Decoupling](#feature-decoupling) section

## Abstraction
-   Something that is more generalized
-   Example:

**Concrete**
```javascript
const Honda = () => {
   this.honda = "Honda";
}
Honda.prototype.display = () => {
   return "Vehicle??is:??" + this.honda;
}
```

**Abstract**
```javascript
const??Vehicle = () => {
   this.vehicleName = "Honda";
}
Vehicle.prototype.display = () => {
   return "Vehicle??is:??" + this.vehicleName;
}
```

## Reusability
-   Make code reusable
-   An example of this would be a header for a website
	-   Instead of typing out a header for each page, create a header component and call it at the top of every page
	-   Example in React:

Header Component
```html
const Header = () => (
	<header>
		<h1>Used-Car Auction</h1>
		<NavLink to="/" activeClassName="is-active" exact={true}>Dashboard </NavLink>
		<NavLink to="/create" activeClassName="is-active"> List Car</NavLink>
	</header>
);
```

Router Component
```html
const AppRouter = () => (
	<BrowserRouter>
		<div>
			<Header />
			<Switch>
				<Route path="/" ... />
				...
			</Switch>
		</div>
	</BrowserRouter>
);
```

## Flexibility
-   Anticipate that your system is going to change in the future
-   The application may be simple now but in the future it may get much more complex
-   A month or a year after you create a project you may need to scale up and if you need to add packages or modules you will need to plan for that ahead of time so you don't have to change the entire code base

## Anticipate Obsolescence
-   Avoid early release and versions of software
-   Use software from reputable companies
-   Use as few external dependencies as possible
-   Avoid poorly documented or maintained projects
-   Be careful of which dependencies you're using as they may become obsolete in the future
    -   Deprecation
    -   No longer work for your specific version of programming language
    -   May not be maintained or supported for some OSes
    -   May not have updates
    -   May have bugs that aren't going to be fixed

## Portability
-   Anticipate that the design or the system you're creating right now may be used om a different platform or device than you're currently targeting
-   If you're making a web app, you may also want to make it a IOS or Android app
-   If you don't factor in portability, it may result in changing the application or creating an entirely new application to port it over

## Testability
-   I go over this in the [Test Driven-Development](#test-driven-development) section.

## Design Defensively

-   Idiot-proof your code
-   Have good error messages
-   Handle all invalid inputs and outputs
-   Handle anything that could go wrong from the user using your code
-   Design it in a way that so that no matter who uses it, it will not break

---

# Web Programming Skills
I've acquired the following certificates:
- CCA HTML Level 1
- CCA CSS Level 1
- CCA JavaScript Level 1
- CCA JQuery Level 1
- [JavaScript Basics for beginners](https://www.udemy.com/certificate/UC-cb18ba63-4c8a-4ffd-af86-5c50b8bef6e0/)
- [The Complete Node.js Developer Course (3rd Edition)](https://www.udemy.com/certificate/UC-b922a00a-beed-4d16-ac60-824e90c93eca/)

**Web apps I made that are currently deployed:**
- [Chat App](https://becnel-nodev3-chat-app.herokuapp.com/)
- [Weather app](https://becnel-weather-app.herokuapp.com/)
- [Expensifiy app](https://react-course-learn-syntax-1.herokuapp.com/)

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

# Technical Debt
Martin Fowler, a software developer whose been in the industry for over three decades, created a simple graph called the **Technical Debt Quadrant** to show how technical debt occurs.

|  | Reckless | Prudent
| :---: | :---: | :---:
| **Deliberate** | "We don't have time to design this" | "We need to ship now and worry about the consequences later"
| **Inadvertent** | "What's layering?" | "Now we know how we should've done it"

According to a research paper titled [???Towards an Ontology of Terms on Technical Debt???](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=312135), which was published by the Software Engineering Institute, there are 13 types of technical debt.

- **Architecture Debt**
	- Issues in software architecture
	- Structural Analysis
		- Analyzing the interdependencies inherent within the code and related systems
	- Structural Dependencies
		- Frameworks, libraries, and other third-party dependencies will become outdated if you don't properly maintain them
	- Violation of Modularity
		- Two modules that are supposed to be independent may always change together, due to unwanted side effects caused by quick and dirty implementation
- **Build Debt**
	- "Dead Flags"
		- Feature Flags that are not used or deprecated
	- "Zombie Targets"
		- A target is marked as ???dead??? if the attempts to build it have failed for at least 90 days
	- Dependency
	- Visibility
- **Code Debt**
	- ASA Issues
	- Code Metrics
		- Size, complexity, coupling, cohesion, and inheritance
	- Code outside of standard
	- Duplicate code
		- Code that's written twice in the same file or environment
	- Multithread correctness (ASA)
		- Typically difficult to design, debug, and prove
	- Slow algorithm
- **Defect Debt**
	- Uncorrected known defects
- **Design Debt**
	- Code Metrics
	- Code Smells
		- Used to capture symptoms of code decay and potential maintenance problems
		- Data Class
		- God class (large class)
		- Brain Method
		- Data clumps
			- Variables that are passed around together throughout various parts of the program
	- Dispersed Coupling
		- Methods of a class that depend on many operations across multiple classes
	- Duplicate Code
	- Grime
		- Accumulation of unnecessary or unrelated software artifacts within classes
	- Intensive Coupling
		- When classes, methods, and/or functions rely too much on one another
	- Issues in the software design
	- Refused Parent Bequest
- **Documentation Debt**
	- Documentation does not exist
	- Incomplete Design Specification
	- Incomplete Documentation
	- Insufficient comments in code
	- Outdated Documentation
	- Test Documentation
- **Infrastructure Debt**
- **People Debt**
- **Process Debt**
- **Requirement Debt**
	- Requirement Backlog List
		- Prioritized list of work for the development team
	- Selection/Replacement of webservice
- **Service Debt**
- **Test Automation Debt**
- **Test Debt**
	- Incomplete Tests
	- Low coverage
		- Tests only cover a small amount of code

---

# Enterprise-level Development

## Business of Architecture
The basic goals of enterprises are to:
- Attract Investment
- Research and Development
- Build business connections
- Employ a plethora of smart individuals
- Compete in the global economy

In large enterprises, developing software can get complicated. Multiple projects being worked on with a large amount of people involved. How do they organize everything?
- Create blueprints for the various projects
	- Design in a way to show how the various "pieces of the puzzle" connect to each other to create the desired outcome
- Make sure everyone understands the goals of the Enterprise
	- A proper business architecture allows important decision makers to operate based upon a shared understanding of the enterprise's goals


## Maps of Business Architecture
**Capability Maps**
- Shows the capabilities that an enterprise possesses

**Information Maps**
- Information that is used to manage the business

**Initiative Maps**
- Equips an enterprise with visibility of all their initiatives
- Helps enterprises strategy with prioritization of which business initiative will yield the best outcome

**Organization Maps**
- Informs enterprises of the various internal departments as well as third-party vendors that are involved in business operations

**Stakeholder Maps**
- Informs the enterprise of the stakeholders that are participating

**Strategy Maps**
- A way for enterprises to develop a thorough plan to make top-level decisions

**Value Maps**
- A way to visualize how value is added to an enterprise


## Technical Architecture
**Infrastructure Architecture**
- Server hardware
	- CPU
	- GPU
	- RAM
	- I/O
	- Network connectivity
	- Power supply
- Storage
	- Storage Networks
	- Storage compression software
	- Storage Media
	- Storage Interface Protocol
- Data Centers
	- Physical Racks for servers
	- Networking cables and cable management
	- Power/electricity
	- Environment Control Systems
- Networking infrastructure
	- WAN
		- Wide-area Network
	- LAN
		- Local-area Network
- End User devices
	- Personal desktops and/or laptops
	- Cell phones
	- Tablets


**Information Architecture**
- Focus on organizing, structuring, and labeling things in an effective way
- Develop information based systems
- A way to organize all the information an enterprise has and evolve it to meet the demands of tomorrow

**Security Architecture**
- The systems in place to guard an enterprise's secrets and information
- Seven Security Attributes
	- Accountability
	- Authentication
	- Access Control
	- Availability
	- Confidentiality
	- Integrity
	- Safety

**Shared Nothing (SN) Architecture**
- A method of distributing computing architecture so that each node is independent of each other and don't share memory with one another

## Cloud Native Applications
**APIs**
- Restful (REST) design
	- REpresentational State Transfer
- Encryption key for access
- JSON data format

**Microservices**
- Dividing an application into a collection of services
	- Maintainable
	- Testable
	- Loosely coupled
	- Independently deployable
- APIs fit them together

## Cloud Computing
- On-demand computer system resources
	- Storage
	- Computing power
- Allows for quick provisioning of computers when the demand arises without having to spend thousands of dollars and spending days or weeks manually configuring computers

## Data Domains
- Can mean either database management or database governance
- Database management
	- Represents the collection of values that a data element may contain
- Database governance
	- A logical grouping of items or areas of interest within an enterprise

## Big Data
- Datasets that are too large or complex to be handled by traditional data processing
- The vast amount of data points allow for enterprise's to process trends, patterns, and links that normally wouldn't be found

**Volume**
- The amount of data that is stored

**Velocity**
- How fast data is collected and processed

**Variety**
- Structured data
	- Can be organized neatly within the columns of a database
- Unstructured data
	- Difficult to sort and extract value from

---

# Version Control in the Software Delivery Lifecycle

Version control, a.k.a source control, is a practice that allows software development teams to track changes to the code and manage those changes quickly. The most common version control tool is *git*, however there are others, such as *Subversion* (SVN), *Mercurial*, and many more.

**The Benefits**
- Quality
	- Software developers can proof-read each others code
- Acceleration
	- Branching code and making changes happens much faster with version control
- Visibility
	- Allows for greater collaboration
- Safety
	- Helps development teams avoid using duplicate code or use outdated versions of the software

## Important Things Version Control Offers
- Concurrent Development
	- Multiple developers and designers have the ability to work on the same files
- Automation
	- [Continuous Integration](#continuous-integration)
- Team Collaboration
	- Allows developers from all over the world contribute to the code base
- Tracked Changes
	- Who made the changes
	- What changes did they make
	- When did they made the changes
	- Why they made the changes
- Availability and Disaster Recovery
	- Have systems in place to have backups of the code base
	- If something happens to the repository you need to have an up-to-date backup
- Baselines, Labels, and Tags
	- Distinct snapshots of the code base that enables teams to navigate throughout the different versions of the software's history
	- Each repository revision is essentially a snapshot of the file system

## Versioning
- The practice of giving a unique number to each state of your code base
	- New projects typically start at version 0.1.0
	- Some projects start at 1.0.0 if:
		- The software is already used in production
		- Need the ability for backwards compatibility
- The process of versioning is useful to keep track of the different versions of code

**Semantic Versioning**
- A better numbering scheme that contains useful information
- Each part of the version number has a defined meaning
- Version 5.1.2 -> X.Y.Z
	- X. = *Major Version*
		- Incremented whenever major architectural changes are made
		- API become incompatible because of the changes
	- Y. = *Minor Version*
		- Incremented whenever minor changes are made
		- Does not break the API like a new feature would
	- Z. = *Patch Version*
		- Incremented whenever there are bug fixes
	- So 5.1.2 means
		- 5th major release
		- 1 minor change was made to the 5th release
		- 2 bug fixes

## Distributed Version Control Systems (DVCS)
- Tracks changes on a local machine
- Each developer has a their own copy of the repository
- Need to push and pull changes to a code hosting service if you want to collaborate with a team
	- GitHub
	- GitLab
	- Bitbucket
- Fully mirrors repository including it's history
	- Each repository clone is a complete backup

## Centralized Version Control Systems (CVCS)
- Forces developers to use a a single shared repository held on a server
- Allows for streamlined collaboration
- Every developer commits to the main branch
- Because of quick communication it prevents multiple developers from working on the same code simultaneously
- Admins have much greater control of who can do what

## Linear Development
- All commits happen one after another
- There are no merges with independent commit histories
- This method keeps a clean history of changes

## Non-linear Development
- Enables teams to create thousands of parallel branches running on multiple systems
- Teams can create and merge branches while navigating backwards and forwards throughout the version history


---

# Understand, articulate, and demonstrate clean coding behaviors

There are many reasons why you should write clean code, the most important reason is the readability of your code. Reading code can be much more difficult than writing code. Whether it's your coworkers, contributors to your open source project, or even yourself if your're refactoring code that you originally wrote weeks ago.

## There are few techniques to writing cleaner code:

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

- Even though this code works, both `thing` variables have no real meaning and the name repeats itself which makes it difficult for other people to understand
- The array with the variable name `num` is the exact same syntax as the value 'num', which can cause confusion when reading `this.num.map((num) => num`
- What does `otherNum` even do?

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
