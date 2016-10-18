---
title: Getting Started With Ajax
length: 180
tags: JavaScript, AJAX, APIs, front-end
---

### Goals

By the end of this lesson, you will know/be able to:

* Make all CRUD functionality AJAX calls.
* Access APIs through all client side JavaScript.
* Dynamically change content on a webpage without reloading the page.
* Explain the difference between client-side and server-side.

### Structure

* 25 - Read and Teach
* 5 - Break
* 5 - Review
* 20 - Code Along - AJAX basics - GET
* 5 - Break
* 25 - Code Along - AJAX - POST DELETE
* 5 - Break
* 5 - Code Along Review
* 20 - Workshop 1: Refresh
* 5 - Break
* 25 - Workshop 2: Polling and Put
* 5 - Break
* 25 - Finish up and Review

#### Why AJAX?

* UX and User Perception
* Client Side interactions - voting, polling, rating, interact with HTML elements, auto-complete, nearly instantaneous loading


### Reading

* [1s - AJAX: History](http://www.phpasks.com/articles/historyajax.html)
* [1s - More AJAX History](http://www.softwareengineerinsider.com/programming-languages/ajax.html#context/api/listings/prefilter)
* [2s - Client Side vs Server Side](http://www.codeconquest.com/website/client-side-vs-server-side/)
* [2s - More Client Side vs Server Side](http://skillcrush.com/2012/07/30/client-side-vs-server-side/)


### Checks for Understanding

* What does AJAX stand for?
* What is the difference between client side and server side?
* What is the main reason we use AJAX?


## Code Along - AJAX ($.ajax - $.getJSON - $.post)

### Repository

Birdeck is a twitter like service were you'll find posts. The homepage, [found here](https://turing-birdie.herokuapp.com), explains what routes you have available. We will use the Birdeck repository. You can find the code below. Make sure you clone the repository Birdeck - `git clone https://github.com/turingschool-examples/birdeck.git`. It is a simple html page which we will use to display and interact with the the Birdeck API.

* Birdeck - https://github.com/turingschool-examples/birdeck.git
* Birdie (third-party API)
  * Repo - https://github.com/turingschool-examples/birdie
  * Heroku - https://turing-birdie.herokuapp.com

## Debugging in Javascript

Debugging JavaScript is a different beast than debugging Ruby. Because JS is run entirely in the browser, the technique for troubleshooting broken code is more complicated than `binding.pry`. Luckily, modern browsers are aware of this and give us a collection of options for digging into your code.

### 1. Developer Tools
One of the first things you should familiarize yourself with when working with JavaScript (or HTML...or CSS...) are the dev tools. You can find a cool tutorial to dive deeper with  [Code School's Discover-DevTools Tutorial.](http://discover-devtools.codeschool.com/) (Chapters 3 & 4 are particularly helpful)

To open developer tools in Chrome:
  - Mac: `Cmd` + `Opt` + `i`
  - (or) Right click on the browser window and select `inspect`
  - (or) Select `View` in the navbar, then `Developer`, then `Developer Tools`

Personally I find that pinning the dev tools to the upper right is the most convenient (You can also expand them into their own window)

![navigating](http://g.recordit.co/L8euYRVfrA.gif)

*Note - To see both the default dev tools AND console at the same time, hit `esc` with the dev tools open. You can also click on the three little dots in the upper right hand corner of the dev tools window and select `Show Console`*

When working with JavaScript, it is useful to keep your console open at all times to watch for errors and anything you've told your code to print out. Bringing us to...

### 2. console.log()
`console.log()` is to JS what `puts` is to Ruby. This line of code will print whatever is provided as an argument to the console.

Given the following function called `printStuff()`, adding console.log() will print the value of `myVariable` to the console.
```
function printStuff(){
  var myVariable = 5 + 5
  console.log(myVariable);
}

printStuff()
=> 10
```

If you're confused about what a variable or function is returning, throw `console.log()` into your code or directly into the `console` in your browser to confirm/deny suspicions.

### 3. Debugging In the Console

There are two ways to use JS debugger.

#### Option 1: Let the Console Debug For You

> For the console is dark and full of errors

Lets say you want to build an app that keeps track of some really great ideas. You wrote a bunch of code to filter said ideas based on text you enter into a search field and as far as you know, everything is perfect.

The following event delegator watches for text to be entered into search field. ( For this example I've intentionally misspelled the reference to the jQuery variable `$ideaContent` to be `$ideacontent` in the if statement.)

```
// index.js
$('#search-ideas').on('keyup', function() {
  var currentInput = this.value.toLowerCase();

  $ideas.each(function (index, idea) {
    var $idea = $(idea);
    var $ideaContent = $idea.find('.content').text().toLowerCase();
    if ($ideacontent.indexOf(currentInput) >= 0) {
      $idea.show();
    } else {
      $idea.hide();
    }
  });
```

When I first fire up the browser, the console is free of errors, and my app looks normal, but when I enter an `s` in the search bar, my ideas aren't filtering correctly. WTF Mate?

If I open my console in the dev tools (`opt+cmd+i`) this is the screen I see:

![Console Error](http://i.imgur.com/sPfEO15.png)

Check it out! There are three important things to notice here. One, we see a little red circle with an X in the corner. This will appear when you are on any tab in the console, but for more details select the `Console` tab.

In our console log history we see a very helpful error message! `$ideacontent is not defined`. That's a true story because we spelled it wrong. We can fix that! To find out exactly where this line of code is found, check out the link to the right. Here it tells us that the error is happening in (`search.self-....js?body=1:11`) - So we're dealing with our `search.js` file, and the last digit `11` indicates the line number.

We see The file name, the chunk of text causing problems, and the exact line where `$ideacontent is not defined` is flagging the debgger. Huzzah! If you click on this link it will open up the `.js` file and point directly to the issue.

![Imgur](http://i.imgur.com/cbcFX0B.png)

#### Option 2: Stick `debugger` Directly Into Your Code
Debugger is the `pry` of JS. Stick `debugger;` within a function to pause the browser from running the script when it hits a particular part of your code.  

Lets revisit our search-functionality example, but this time throw the line `debugger;` where you think you're having a problem (just like using `binding.pry`).

```
// index.js
$('#search-ideas').on('keyup', function() {
  var currentInput = this.value.toLowerCase();

  $ideas.each(function (index, idea) {
    var $idea = $(idea);
    var $ideaContent = $idea.find('.content').text().toLowerCase();
    debugger;
    if ($ideacontent.indexOf(currentInput) >= 0) {
      $idea.show();
    } else {
      $idea.hide();
    }
  });
```

In the browser, if we open up the dev tools, navigate to the console and try to search for something.  The program will freeze on the line `debugger`.

*NOTE - The console must be open for debugger to catch, otherwise the app will look normal and you won't get any error messages - if you get stuck, refresh your page while the console is open and go from there.*

![Imgur](http://i.imgur.com/Ch2bxhb.png)

You can see the code has stopped on the line `debugger`. This lets us type some stuff into our `console` to see what's going on. Here I've copied the following line to see if that's where the issue. SURPRISE! `$ideacontent` is still undefined.

The two areas marked with `***` are the js control panel - Here you can either click the blue play arrow to continue executing the function (similar to  `exit!` in `pry`, which is the blue arrow) or "step through" to the next action in that function (the arrow over the dot).  

___

For more details and information about other ways to dig into your js, check out the [Chrome Documentation](https://developer.chrome.com/devtools/docs/javascript-debugging).


## Workshops

### Workshop 1: Refresh

1. Right now, you have to reload the page to get the new posts. Can you modify your app so that the posts are reloaded when you click a button?
2. Right now, there is some duplication when we render the template, can you refactor?

### Workshop 2: Polling & PUT

1. Right now, you have to click on a button to refresh the feed. Can you check the server every 5 seconds to see if there are any new posts?
2. Can you implement a PUT option to modify the data via AJAX?

### Additional Resources

* [How to use $.ajax()](http://www.sitepoint.com/use-jquerys-ajax-function/)
* [Basics of Jquery AJAX](http://www.i-programmer.info/programming/jquery/8895-getting-started-with-jquery-ajax-the-basics.html)
* [jQuery Promises and Deferred](http://www.i-programmer.info/programming/jquery/4788-jquery-promises-a-deferred.html)
* [MDN AJAX](https://developer.mozilla.org/en-US/docs/AJAX)
* [MDN Getting Started with AJAX](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started)
* [jQuery $.ajax()](http://api.jquery.com/jquery.ajax/)

### Video

* [Video](https://vimeo.com/131025914)
