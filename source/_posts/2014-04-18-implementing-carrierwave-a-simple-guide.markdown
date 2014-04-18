---
layout: post
title: "Implementing CarrierWave - A simple guide"
date: 2014-04-18 13:55:53 -0400
comments: true
categories: carrierwave, flatrion school, coding, ruby, rails, gem, uploading
---

This weekend I decided to add file uploads to a small project I’ve been working on (mostly to get some additional rails practice). After searching for a while, I came across Carrierwave, which is a simple ruby gem that sets you up with easy image uploading and processing. 

The setup was a lot easier than I expected. First step was to add the carrierwave gem to the project’s gemfile and run ‘bundle install’. Straightforward. After this, run ‘rails generate uploader {uploader_name}’ (in my case, the name was guests (as in hotel guests). In your app file you’ll now find an ‘uploaders’ directory, in which you will find the uploader class that you just generated. Cool. Leave that there for now.

You’ll need to add a column to the ActiveRecord model that will hold your image. We’ve done this before: ‘rails generate migration Add___to____ image:string’. Run your migrations to get this into the database, and then head over to the model, where you’ll mount the uploader:
```ruby
mount_uploader :image, ImageUploader
```
Great. You’re almost there. The last piece of the puzzle is getting the upload link for the image into your form. Rails, as always, comes to the rescue with a file upload tag:
```html
  <p>
    <%= f.file_field :image %>
  </p>
```
That’s pretty much it. There are a bunch of additional settings you can change in the ImageUploader class - The ability to scale the image, to create various versions (thumbnails, different sizes, etc) and settings to determine where the image gets saved. I have this running locally now, but I imagine that once I get this up I’ll have to set the storage to AWS or something similar.

Peace.

Resources:

http://railscasts.com/episodes/253-carrierwave-file-uploads

https://github.com/carrierwaveuploader/carrierwave