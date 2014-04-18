---
layout: post
title: "Removing an Item with the Jquery-CSS-Ajax Trifecta"
date: 2014-04-18 14:27:04 -0400
comments: true
categories: ajax, jquery, css, removing images, flatiron school
---

Recently I've been working on a basic dive logging application with my buddy Chris from caguthrie.com. The idea is to take the very manual booklet that divers have to fill out after a dive and bring it online. Similar products like this exist, but we wanted to make our own. 

I'm using this project to beef up my javascript/ajax skills and as such, will be writing today about removing items from a page using javascript, and then removing those items from your database by making an ajax request to your server.

We built a section in the log where you can add fish that you've seen on your dive. It's based on a scrape of common fish from wikipedia. Here's an example:

->{% img /images/ajaxpic1.png %}<-

Now let's say I didn't actually see a manefish on my most recent dive. How would I remove it? We need to figure out the following steps:

+ Display a remove button when you hover over a fish.
+ Remove the fish when the remove button is pressed.
+ Send a request to the server to remove the association between the dive and the fish.

Let's go in to each of these steps in more detail:

**Part 1 - Display a Remove Button on hover using CSS:**

->{% img /images/ajaxpic2.png %}<-


Displaying a button on hover can be done in either javascript or css. In our case, we went with the more straightforward css. We created a button span with a specific class ("fish-remove)" that we placed withing the fish thumnail div. The button also contained data about the dive and the fish id (we added this data with ERB). Here is the html(with erb) for each fish 'card':

```html
<div class="col-md-6">
    <a href = "<%= fish.wiki_link %>" target="_blank">
	    <div class="thumbnail fish-thumb">
	    	<button class="btn fish-remove" data-fish-id=<%= fish.id %>, data-dive-id=<%= @dive.id %>>X</button>
	      <%= image_tag "http://#{fish.picture_link}" %>
	      <div class="caption">
	        <h4><%= fish.name %></h4>
	      </div>
	    </div>
	  </a>
  </div>
```

 We hid the button using css:

```css
.fish-remove {
	visibility: hidden;
	position:absolute;
	top:5px;
	right:20px;
	margin:0;
	padding:5px 3px;
}

```

You can see that we set the position to absolute and made some changes to margin, padding, etc. This was all done to improve the positioning of the button - you'll have to play with this yourself.

Next, we set the visibility for the .fish-remove class to become visible when you hover over it's parent .fish-thumb class:

```css
.fish-thumb:hover .fish-remove{
	visibility: visible;
}
```

At this point, if you test your page out, you'll have the button hidden and and showing up when you hover over it. But clicking on it won't do anything.

**Part 2 - Using JQuery to remove the thumbnail from the page**

Great. We have a working button. Now, let's write some jquery code to listen for a click on the remove button and remove the thumbnail:

```javascript
	$("#fish").on("click", ".fish-remove", function(event){
		event.preventDefault();
    $(this).closest(".thumbnail").remove();
	});
```

What did we do here? We created an event listener (.on "click") that listens for a mouse click on any ".fish-remove" class within the #fish id. We did this so that it will work regardless of whether items have been added to the page via agjax or not. We prevent default on the button and then remove the closest parent .thumbnail class from the page.

Reload your page and you'll see that you can now remove the image when you click on the button. Stupendous. But if you reload again, you'll see the image is still there. That's because you haven't actually send a request to the server to remove the association between the image and the dive. We'll do that next, in part three.

**Part 3 - Using ajax to remove a database association.**

If you remember, in part 1 we added additional data in to our html button element: the fish id and the dive id. We 
did this so that we can pull this information in to our javascript and send it in our a jax request. To get this data in our js file, use the .data method with the attribute names that you assigned. He we set them to variables for later use:

```javascript
var id = $(this).data('dive-id');
var diver_id = $(this).data('diver-id');
```

We also set the current item that has been selected ($(this)) to a variable for later use:

```javascript
var currentX = $(this);
```

Now, let's write the ajax request that will send  the data to the server. I've created a RESTful route in my routes.rb file that looks like this:

```ruby
  delete '/dives/:id/fish/:fish_id' => 'dives#removefish'
```

So we need to send a delete request with the dive id and the fish id. Now you see why we have that information as variables!

So, the Ajax:

```javascript
		$.ajax({
			url: '/dives/'+id+'/divers/'+fish_id,
    	type: 'POST',
    	data: {"_method":"delete"},
    });
```

This will remove the fish from the dive on the backend. Let's take the frontend solution we developed in part 2 and add it in to the javascript when we receive a  response from the server telling us that it successfully removed the fish:
```javascript
		$.ajax({
			url: '/dives/'+id+'/fish/'+fish_id,
    	type: 'POST',
    	data: {"_method":"delete"},
    	success: function(event){
    		currentX.closest(".thumbnail").remove();
    	}
    });
```

Amazing! Put it all together, and voila:

```javascript
//Removing a fish
	$("#fish").on("click", ".fish-remove", function(event){
		event.preventDefault();
		var id = $(this).data('dive-id');
		var fish_id = $(this).data('fish-id');
		var currentX = $(this);


		$.ajax({
			url: '/dives/'+id+'/fish/'+fish_id,
    	type: 'POST',
    	data: {"_method":"delete"},
    	success: function(event){
    		currentX.closest(".thumbnail").remove();
    	}
    });
	});
//end removing a fish
```

Make sure you have a removefish method in your dives controller that renders "render :json => { :head => :ok }" when the process of removing the fish form the dive is complete. This is what your javascript will receive as the success response. Here's the method from our controller for reference.

```ruby
 def removefish
    @dive = Dive.find(params[:id])
    @fish = Fish.find(params[:fish_id])
    @dive_fish = DiveFish.find_by(:dive_id => @dive.id, :fish_id => @fish.id)
    @dive_fish.destroy
    render :json => { :head => :ok }
  end
```

Wow. That was a long blog post. Hopefully this is useful to Javascript/Ajax beginners!

