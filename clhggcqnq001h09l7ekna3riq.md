---
title: "Use localStorage to power up your apps"
datePublished: Tue May 09 2023 15:54:32 GMT+0000 (Coordinated Universal Time)
cuid: clhggcqnq001h09l7ekna3riq
slug: use-localstorage-to-power-up-your-apps
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/TluMvvrZ57g/upload/79c2c77c51c0f3002cb7335048936a33.jpeg
tags: programming-blogs, javascript, web-development, developer, wemakedevs

---

### What is localStorage?

Web pages do not persist data by default. If you reload the page while writing a long text, the text is gone forever. This lack of persistence could be very annoying in situations like:

* Setting the page to dark mode, but having it revert to light mode on return.
    
* Spending time choosing products for a shopping cart, only to find it empty when you return.
    
* Writing a lengthy blog post, being logged off, and losing all progress when the post is submitted
    

Using `localStorage` enables your apps to save information without the need for a backend server. This means no need for storage infrastructure and no costs for you since the data is being stored on your user's computer instead of a server you manage.

It also can be used for static pages to have data persistence. Imagine you built a simple ToDo app that is hosted on Github Pages. It is fast to load and can be hosted for free, but it is useless because when the user leaves the page, all his tasks are gone. `localStorage` can solve this issue by saving the tasks in the user's machine.

### Here's how to use localStorage

* `setItem` saves data to the user's browser
    
* `getItem` retrieves the saved data from the user's browser
    
* `removeItem` removes the saved data from the user's browser
    

### Example 1: Dark mode

Suppose I have a webpage that has a dark mode option. I would want the webpage to remember the user's preference. Notice that when retrieving data from localStorage, `getItem()` always returns the data as a string, so you need to convert it to the actual data type that you originally saved using `JSON.parse()` to avoid using true as a string in a comparison, which could lead to errors in the app:

```javascript
// Store the user's preference
localStorage.setItem('darkMode', true)

// Retrieve the user's preference from localStorage 
// getItem() always returns a string
const darkModeData = localStorage.getItem('darkMode') // returns a string: 'true' 

// Convert string to a Boolean value
const darkMode = JSON.parse(darkModeData) // returns a bollean: true
```

### Example 2: A static page

In the second example, I have a To-Do app and I want to save my list of tasks to do. I can save the array in localStorage, but when saving complex data structures like objects and arrays, I need to convert them to a string using `JSON.stringify()`:

```javascript
const tasks = [
    'Go to gym',
    'Prepare dinner',
    'Go to supermarket',
]

// Array tasks gets converted to a string
arrayAsString = JSON.stringify(tasks)

// Store the array as a value of the key tasks
localStorage.setItem('tasks', arrayAsString)
```

### Why do I need JSON.stringify()?

You could be thinking that `localStorage.setItem()` saves the value as a string anyways, so why the need to convert the value to a string beforehand using `JSON.stringify()` ?

That is because behind the curtains `localStorage.setItem()` uses `toString()` which gives some unexpected results compared to `JSON.stringfy()`:

```javascript
/* With booleans, integers and float the result is the same */
const boolExample = true

boolExample.toString() 
// Returns 'true'

JSON.stringify(true) 
// Returns 'true'

/* When using objects, the results are very different */

const product = {
  name: 'ice cream',
  price: 1.20,
  quantity: 2
}

product.toString()
// Returns '[object Object]'

JSON.stringify(product) 
// Returns '{"name":"ice cream","price":1.2,"quantity":2}'

/* With arrays, there are unexpected differences */

const users = ['Paul', 'Maria', 'Ken']

users.toString()
// Returns 'Paul,Maria,Ken'

JSON.stringify(users)
// Returns '["Paul","Maria","Ken"]'
```

In summary, `JSON.stringify` converts objects and arrays into strings in a way that it is possible to easily convert them back to their original forms. On the other hand, `toString()` removes their original data structure, making it difficult or impossible to convert back to their original form.

### Example 3: A shopping cart

```javascript
const cartItems = [
    {
        product: 'ice cream',
        flavour: 'vanilla',
        quantity: 1    
    },
    {
        product: 'hamburger',
        toppings: ['picles', 'cheese'],
        quantity: 2
    }
]

// Same as before, convert the array of object into a string
arrayAsString = JSON.stringify(shoppingCart)

// Store the array of objects converted to string
localStorage.setItem('cartItems', arrayAsString)

// Deletes the cart
localStorage.removeItem('cartItems')
```

## Conclusion

`localStorage` is a great way to store data without the need to set and maintain databases. You are transferring the responsibility of storing data from your servers to your users, so you can power up your static websites or can expand your page's capabilities without touching your database servers.

The data stored at `localStorage` will not expire and will remain as long as the user does not clean the browser's cache. Depending on the browser, it can store up to 5MB of data or an unlimited amount.

You should never use `localStorage` for storing sensitive data, such as passwords, login sessions or credit card information, because the data stored in `localStorage` is not encrypted and can be accessed by a malicious hacker or even someone who has access to the browser can just use the browser's developer tools and see what is stored there.

Happy coding!