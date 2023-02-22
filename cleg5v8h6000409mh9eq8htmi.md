# Use ChatGPT as your free programming teacher

Programming is complex and hard. Feeling overwhelmed by a bug is something that newbies and experienced programmers can relate to. If only you could ask someone more experienced...

[Stack Overflow](https://stackoverflow.com/) is a great resource, but it requires to search through the answers to find the best solution or to visit multiple related questions to find the answer to a specific problem. Its moderation rules are very strict, so open-ended questions are banned, and questions that are considered similar to already existing ones are quickly closed. It is intended to use a simple and direct input =&gt; output style forum, where a users inputs a question and the community answers the question and the most up-voted answer becomes the standard answer for the question.

[ChatGPT](https://en.wikipedia.org/wiki/ChatGPT) was released in 2022 and caused a great uproar, as it is an AI that can produce answers in many domains of knowledge, in a very human and smart way. It is not perfect, but it is expected to do huge leaps of evolution since it is constantly learning from its 100 millions of users and the ever growing data on the Internet. You can test it for free on its [official page](https://openai.com/blog/chatgpt/).

The launch to the general public of such polished AI for free created huge shockwaves across the tech industry and [caused Google to panic](https://www.theverge.com/2023/1/20/23563851/google-search-ai-chatbot-demo-chatgpt), as ChatGPT was integrated into Microsoft's search engine Bing, creating a much more intelligent and human like search experience, threatening Google's search business, which is the main source of revenue for the tech giant.

## Examples of use

ChatGPT can answer in a detailed way almost all kinds of programming questions:

> **Me**: I have an array of objects in Javascript in which each object has a boolean value. How do I filter objects based on this value?
> 
> **ChatGPT**: To filter an array of objects in JavaScript based on a boolean property, you can use the `filter()` method along with a conditional statement that checks the value of the property. Here's an example:
> 
> ```javascript
> const myArray = [
>   { id: 1, value: true },
>   { id: 2, value: false },
>   { id: 3, value: true },
>   { id: 4, value: false }
> ];
> 
> const trueValues = myArray.filter(object => object.value === true);
> const falseValues = myArray.filter(object => object.value === false);
> 
> console.log(trueValues);
> console.log(falseValues);
> ```
> 
> In this example, we have an array of objects with an `id` property and a `value` property that contains a boolean value. We want to filter the objects that have a value of `true` and the objects that have a value of `false`.
> 
> We use the `filter()` method twice, once to filter the objects that have a value of `true`, and once to filter the objects that have a value of `false`.
> 
> In each call to `filter()`, we pass a callback function that takes an object as its parameter. The callback function checks the `value` property of the object using a conditional statement, and returns `true` if the value matches the condition, and `false` otherwise.
> 
> The `filter()` method returns a new array that contains the objects that match the condition. We store the two resulting arrays in separate variables and log them to the console.

You can refer to its previous answer to ask new questions:

> **Me**: Is it possible to filter the value in a single loop?
> 
> **ChatGPT**: Yes, it is possible to filter an array of objects based on a boolean property using a single loop. Here's an example:
> 
> ```javascript
> const myArray = [
>   { id: 1, value: true },
>   { id: 2, value: false },
>   { id: 3, value: true },
>   { id: 4, value: false }
> ];
> 
> const trueValues = [];
> const falseValues = [];
> 
> for (let i = 0; i < myArray.length; i++) {
>   const object = myArray[i];
>   if (object.value === true) {
>     trueValues.push(object);
>   } else {
>     falseValues.push(object);
>   }
> }
> 
> console.log(trueValues);
> console.log(falseValues);
> ```
> 
> In this example, we initialize two empty arrays, `trueValues` and `falseValues`, to store the objects that match the condition.
> 
> We then use a `for` loop to iterate over the array of objects. For each object, we check the `value` property using a conditional statement, and push the object to the corresponding array based on its value.
> 
> The `for` loop allows us to filter the array in a single pass, without having to call the `filter()` method twice. We store the resulting arrays in separate variables and log them to the console.

It is well aware of the context in which the questions are asked and it is not just a database of answers of StackOverflow:

> **Me**: Which solution is the most efficient?
> 
> **ChatGPT**: In terms of performance, the two solutions have different tradeoffs, and the most efficient approach will depend on the specific use case.
> 
> Using the `filter()` method is a more concise and expressive way to filter an array of objects based on a property. It can make the code easier to read and maintain, especially for more complex filtering conditions.
> 
> However, the `for` loop approach can be more efficient in terms of performance, especially for large arrays. This is because it only iterates over the array once, whereas the `filter()` method creates a new array and iterates over it.
> 
> Additionally, using the `for` loop approach allows you to immediately store the objects in separate arrays, which can be more efficient than creating a new array for each filtering condition using the `filter()` method.
> 
> Overall, if performance is a critical concern and the array is very large, using a `for` loop to filter the array in a single pass is likely to be the most efficient approach. If the array is small or performance is not a critical concern, using the `filter()` method can provide a more concise and expressive solution.

It is possible to paste a particular piece of code and ask it if there is an error. Or ask an open-ended question like:

> **Me**: Could you create HTML and CSS code for an app that allows animal shelters to show which animals are available for adoption and show their photo, age, characteristics and current location?
> 
> **ChatGPT**: Sure, here is an example HTML and CSS code for an app that allows animal shelters to show which animals are available for adoption, with their photo, age, characteristics, and current location:
> 
> HTML Code:
> 
> ```xml
> <!DOCTYPE html>
> <html>
> <head>
> 	<title>Animal Adoption App</title>
> 	<link rel="stylesheet" type="text/css" href="style.css">
> </head>
> <body>
> 	<header>
> 		<h1>Animal Adoption</h1>
> 	</header>
> 
> 	<main>
> 		<section class="animal-list">
> 			<h2>Available Animals</h2>
> 			<ul>
> 				<li>
> 					<img src="https://via.placeholder.com/150" alt="Cat">
> 					<h3>Fluffy</h3>
> 					<p>Age: 2 years</p>
> 					<p>Characteristics: Playful, friendly</p>
> 					<p>Current location: Cat House 1</p>
> 					<button class="adopt-btn">Adopt Me!</button>
> 				</li>
> 				<li>
> 					<img src="https://via.placeholder.com/150" alt="Dog">
> 					<h3>Buddy</h3>
> 					<p>Age: 3 years</p>
> 					<p>Characteristics: Energetic, loyal</p>
> 					<p>Current location: Dog House 2</p>
> 					<button class="adopt-btn">Adopt Me!</button>
> 				</li>
> 				<li>
> 					<img src="https://via.placeholder.com/150" alt="Rabbit">
> 					<h3>Bugsy</h3>
> 					<p>Age: 1 year</p>
> 					<p>Characteristics: Cute, cuddly</p>
> 					<p>Current location: Rabbit House 1</p>
> 					<button class="adopt-btn">Adopt Me!</button>
> 				</li>
> 			</ul>
> 		</section>
> 	</main>
> 	
> 	<footer>
> 		<p>&copy; 2023 Animal Adoption App. All rights reserved.</p>
> 	</footer>
> </body>
> </html>
> ```
> 
> CSS Code:
> 
> ```css
> body {
> 	font-family: Arial, sans-serif;
> 	margin: 0;
> 	padding: 0;
> }
> 
> header {
> 	background-color: #008CBA;
> 	color: #fff;
> 	text-align: center;
> 	padding: 10px;
> }
> 
> h1 {
> 	margin: 0;
> 	font-size: 36px;
> }
> 
> main {
> 	padding: 20px;
> }
> 
> .animal-list {
> 	margin-top: 20px;
> }
> 
> .animal-list h2 {
> 	font-size: 24px;
> 	margin-bottom: 10px;
> }
> 
> .animal-list ul {
> 	list-style: none;
> 	margin: 0;
> 	padding: 0;
> 	display: grid;
> 	grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
> 	grid-gap: 20px;
> }
> 
> .animal-list li {
> 	display: flex;
> 	flex-direction: column;
> 	justify-content: space-between;
> 	align-items: center;
> 	border: 1px solid #ccc;
> 	padding: 10px;
> }
> 
> .animal-list img {
> 	max-width: 100%;
> }
> 
> .adopt-btn {
> 	background-color: #f44336;
> 	color: #fff;
> 	border: none;
> 	padding: 5px 10px;
> 	border-radius: 5px;
> 	cursor: pointer;
> }
> ```
> 
> This code creates a simple app with one section, "Available Animals". The section displays a list of available animals with their photo, name, age, characteristics, current location, and an "Adopt Me!" button. The animals are displayed in a grid format with a responsive design.

I did not ask for the page to be mobile-friendly, but Chat GPT did it anyways by using CSS Grid. If you are curious to see how it looks, check it out on CodePen [here](https://codepen.io/edumats/pen/dyqMEPW). It does not look impressive at first sight, but you can certainly give ChatGPT some directions on how to improve it!

## Problems

I noticed that if I asked ChatGPT for a solution and on top of it, added an constraint that it is not possible to fullfill, it can occasionaly produce wrong answers because it is too focused in trying to produce a valid answer:

> **Me**: How can I write a function that converts a float into an integer?
> 
> **ChatGPT**: You can use the `Math.floor` function to convert a float to an integer. This function will round down to the nearest integer.
> 
> Here's an example function:
> 
> ```javascript
> function floatToInt(floatNum) {
>   return Math.floor(floatNum);
> }
> ```
> 
> You can call this function and pass in your float as an argument. The function will return the integer equivalent of the float.
> 
> **Me**: Can your write another option that does not lose precision?
> 
> **ChatGPT**: Sure! You can also convert a float to an integer without losing precision by using the `Math.trunc` method. `Math.trunc` simply removes the decimal portion of a number without performing any rounding.
> 
> Here's an example function:
> 
> ```javascript
> function floatToInt(floatNum) {
>   return Math.trunc(floatNum);
> }
> ```
> 
> You can call this function and pass in your float as an argument. The function will return the integer equivalent of the float without losing precision.

`Math.trunc` does not make any rounding, but still loses precision, as it simply cuts the decimal parts of a float, but ChatGPT does not see a problem in it.

Curiously, it admits the error when confronted:

> **Me**: In the last response, using Math.trunc still loses precision, as it simply cuts the decimal
> 
> **ChatGPT**: You're right, I apologize for my mistake. When truncating a float to an integer, it will always lose the decimal portion of the number, and hence lose precision. The `Math.trunc` function only removes the decimal portion without performing any rounding, but this still results in the loss of the decimal information.
> 
> If you need to preserve the decimal information, you can convert the float to a string and then use the `parseInt` function to extract the integer part of the number.
> 
> Here's an example function:
> 
> ```javascript
> function floatToInt(floatNum) {
>   const floatString = floatNum.toString();
>   const integerString = floatString.split('.')[0];
>   return parseInt(integerString, 10);
> }
> ```
> 
> In this example, we first convert the float to a string using the `toString` method. Then we split the string on the decimal point and keep only the integer part of the number. Finally, we use the `parseInt` function to convert the integer string back to an integer.
> 
> You can call this function and pass in your float as an argument. The function will return the integer equivalent of the float, with the decimal portion discarded, but without losing precision.

But its answer still presents a contradiction, as its solution is still discarding the decimal information while saying that it is preserving it.

## Disadvantages

* It can produce completely wrong answers, so the user must have some level of knowledge to be able to filter down the wrong ones
    
* Can create a bad habit of just copying and pasting its answers, instead of understanding the why's
    

## Advantages

* Free of charge
    
* Can be a very helpful assistant for developers, from begginers to experienced ones
    
* The experience is very close to having a human mentor
    
* Can understand most programming languages, libraries or frameworks
    

## Final Thoughts

AI is not able to produce a beautiful app or a revolutionary startup just yet, but it can become a valuable tool in a programmer's tool box, just like Google or Stack Overflow. We can surely expect more mind blowing surprises from ChatGPT and its competitors in the coming years. The AI wars have just begun!

Have you tried ChatGPT? Feel free to share your impressions about it.