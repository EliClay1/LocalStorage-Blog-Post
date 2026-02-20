## Storing, accessing, and displaying JSON data in local storage.

Have you ever been to a website that saved your data when you left the page? Have you ever questioned what the heck is going on underneath the hood of the site to make that possible? Maybe it’s the yummy cookies or that FBI agent who’s got your back covered. Today’s little info-sesh is going to be on the magic of local storage; the sorcery that happens under the hood of the website to keep hold of your data.

Local Storage is a cache within your browser that can act as a temporary save-state. For the non-technical readers, it’s a box that holds information until you do something like uninstalling your browser. Local storage is probably the most rudimentary form of data storage that we can get our hands on, and what it lacks in persistence it makes up in charm. Utilizing JavaScript, we can store, read, and display JSON data.

What is JSON? Google it. Dumbed down and long story short-ed, it’s a line of text that can easily be interpreted by pretty much anything. Browsers, networks, me, you, and more. A unique property of JSON is that it can be queried from and converted to an object. What is an object? Bruh. This might be a bit too advanced for you… An object is like a digital blueprint. It has values and methods that you can access easily. And that is all the decapsulation we will be doing today. Google things you don’t understand. Use [this link](https://letmegooglethat.com/?q=How+do+I+google+something%3F) if you’re struggling :D
### The Upsides, and the Down.

If you have ever worked with a database before, you’ll know that you can store several data formats. For example, you can typically store dates, integers, bools, longs, lobs (large objects), varchars, and more. If it helps, it may be easier to think of local storage as an extremely high-level database, storing data in the JSON string format.

So then, why name this section the upsides and the down? Because, my dear friend, how many things can you really store in a single string? Plot twist, not a lot. When it comes to bulk amounts of data, it’s not smart to use JSON, or local storage for that matter. This is why local storage is perfect for storing smaller data, like remembering what you typed into a search bar, or remembering what kind of tasks you loaded into a website. I’d like to argue that a good rule of thumb is if the data requires extensive brain power to comprehend, then you probably shouldn’t use local storage.

And now, the moment you all have been waiting for… 
### How to use it!

The magic happens with the `localStorage` command. So go ahead and open up your favorite IDE (I will be using `VScode` for this demonstration) and create a new `.js` file. Let’s create a new function and call it `storageAccess`. It is within that function that we will access the local storage. When you type out the line `localStorage.`, you have a few options of methods to access. I will real quickly run you through what each one does.

`clear`: will clear your local storage.

`getItem`: will get information from your local storage when a key is inputted.

`key`: returns the key value when an index is inputted.

`removeItem`: removes an item from your local storage.

`setItem`: set an item into your local storage that is assigned to the input key.

For those who are confused as to what a `key` is, it is a string that is assigned to a singular keyhole, or what we call a `value`. That value is your JSON string, and it can have all sorts of information stuffed inside. So, to quickly recap, `key → value`. To get the value, you need to know the key.

Another thing that you will want to know / understand is what `localStorage` understands versus what JavaScript understands. Thankfully, our programming ancestors and overlords made it simple for us to convert between these two modes. By typing `JSON.stringify()`, you can turn any valid string into a JSON string, and by typing `JSON.parse()` you can turn any valid JSON string into an object. This is **important**, so **pay attention**. When adding new values to your `localStorage`, you need to use `JSON.stringify()`. When trying to access that information, you need to use `JSON.parse()`. Now write that 20 times on the white board so you don't forget it.
#### Adding things to the local storage

For this example, our key will be called `job_roles` What you are going to want to do is type
``` JavaScript
localStorage.setItem('job_roles', JSON.stringify({"jobs": ['baker','delivery driver', 'manager']}));
```
As you can see, the `job_roles` key is being placed in the `setItem()` function. What this code is doing, is it is taking a JSON object (`{"jobs": ['baker','delivery driver', 'manager']}`) and it is using the `JSON.stringify()` function to convert it into string-form so that the local storage can understand it. 
As I explained earlier, you can access JSON object values using `.`’s so if your JSON string is:
```JSON
{
	"jobs": ["baker", "delivery driver", "manager"]
}
```
Then you can use code like `jobs[0]` on it's object form and it will return the string ‘baker’. Now, back to the example!
#### Reading things from local storage

Now that we have some information within our local storage, we can write some code to access it. 
```javaScript
let storage = localStorage.getItem('job_roles');
```
The following code will create a new variable which is the JSON string that was stored in the local storage. It uses `getItem(<key>)` to grab hold of this data, where our key is `job_roles`. However, in order to really make any use out of this information, we need to convert it into a JSON object, not a JSON string. to do this, you are going to use `JSON.parse(<JSON string>)`. Combining these together, you can create code that looks like this:
```javaScript
function storageAccess() {
    localStorage.setItem('job_roles',
    JSON.stringify({"jobs": ['baker','delivery driver', 'manager']}));
    
    let storage = localStorage.getItem('job_roles');
    
    console.log(JSON.parse(storage).jobs[0]);
}
```
To quickly summarize, this code is setting a JSON string to the key `job_roles`, and then it is creating a new variable that gets hold of that JSON string and stores it in 'storage'. It then uses `JSON.parse(storage)` to convert that string into a JSON object. Lastly, it accesses the first array value of jobs and logs it to the console. A very simple example, but it goes to show just how powerful this can be.

Along with it being a simple example, it's not very practical. So the next thing we are going to do is write some code that gives some application to this! Have you every been writing something into a website, and then suddenly your internet goes out, or your computer dies? It's super annoying when all that text just disappears and you have to retype it. Well, look no further because in this next demo, we are going to create a system that saves your text as you type it! 

The following example is a bit more complicated, but the core ideas remain the same. Feel free to follow along!

#### Applying it all!

Step 1 is going to be creating a simple text input box in html. Feel free to copy this code.
```html
<body>
    <textarea name="text_box" id="text"></textarea>
</body>
```
This is just a very simple html block that has a `textarea` tag with a name and id. If you load this up on your browser, you will find that it's just an empty text box. Nothing too special going on here. The next step is to create a new JavaScript file, which we will call `script.js`. within that file, we need to grab hold of the `textarea` element using the following code:
```javascript
const text_area = document.getElementById('text');
```
now that we have the `textarea` element as a  JavaScript object, we are going to want to toss on an event listener. An event listener just listens for a type of action happening on the page. Like a keyboard input.
```javascript
text_area.addEventListener('input', (e) => {
    const currentText = e.target.value;
})
```
The above code is more complex, but to simplify, that `current_text` variable is whatever is currently in our HTML text box. Now that we have that text, we can start writing to... You guessed it, the local storage! Within that same function, we are going to add some code to write to the local storage
```javascript
text_area.addEventListener('input', (e) => {
    const current_text = e.target.value;
    localStorage.setItem('text_cache', JSON.stringify(current_text));
})
```
The only new line here is the `localStorage.setItem`, where we have set the key to `text_cache` and we do `JSON.stringify()` to convert the text into something that the website can understand. With a single string case like this, converting it with JSON isn't required, but it's a good thing to use either way.

Next, we need to make sure that the site loads the information when you reload the page. We will create the following function to do this:
```javascript
function loadCache() {
    let cache = JSON.parse(localStorage.getItem('text_cache')) || "";
    text_area.innerText = cache;
}
```
So what is this doing? It is grabbing the local storage value assigned to the key `text_cache`, and then converting it into a JSON object. OR it is returning a blank string (if nothing exists in the local storage assigned to that key). Then it sets the text area to whatever value `cache`is. We are almost done!

The last step is going to be adding a window event listener, so this function gets called whenever the page is reloaded.
```javascript
window.addEventListener('load', loadCache);
```
with that final function, the entire script should look like this:
```javascript
const text_area = document.getElementById('text');

text_area.addEventListener('input', (e) => {
    const current_text = e.target.value;
    localStorage.setItem('text_cache', JSON.stringify(current_text));
})

function loadCache() {
    let cache = JSON.parse(localStorage.getItem('text_cache')) || "";
    text_area.innerText = cache;
}

window.addEventListener('load', loadCache);
```
Once all this code is in place, your text will now save across browser closes and site reloads! I hope you enjoyed this quick info-dive into local storage, feel free to share anything cool you make with it! That's all from me, have a great day!

## Additional Resources:

1. [https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API): This is the “official-ish” deep dive on the Web Storage API (localStorage and sessionStorage). It walks through what these APIs are, how they differ, and common patterns for storing key–value data in the browser. Great if you want to sanity-check that your mental model of localStorage isn’t secretly cookies in a trench coat.
    
2. [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON): MDN’s JSON page is the canonical “what even is JSON” reference. It covers the actual spec, plus the `JSON.stringify()` and `JSON.parse()` methods you’re using, with lots of examples. Perfect for when you want to go one level deeper than “it’s a string thing computers vibe with.”
    
3. [https://blog.logrocket.com/storing-retrieving-javascript-objects-localstorage/](https://blog.logrocket.com/storing-retrieving-javascript-objects-localstorage/): A practical article that shows how to store and retrieve full JavaScript objects in localStorage using JSON, including edge cases and best practices. Think of it as “your assignment, but with more production-y caveats” like handling missing data and avoiding silent failures.[](https://blog.logrocket.com/storing-retrieving-javascript-objects-localstorage/)​
    
4. [https://www.tiny.cloud/blog/javascript-localstorage/](https://www.tiny.cloud/blog/javascript-localstorage/): A friendly walkthrough of localStorage with small, focused examples (including saving editor content). It pairs nicely with your textarea autosave demo and shows how people actually wire this up in slightly larger apps. Good for grabbing ideas without getting buried in theory.[](https://www.tiny.cloud/blog/javascript-localstorage/)​
    
5. [https://www.youtube.com/watch?v=W4tCRwlAZGg](https://www.youtube.com/watch?v=W4tCRwlAZGg): A video tutorial that does a full tour of localStorage, including storing JSON, using `stringify`/`parse`, and managing data over time. If you prefer to have someone talk you through the concepts while you code along, this is an easy “hit play, copy the patterns, pretend it was all your idea” resource.​
