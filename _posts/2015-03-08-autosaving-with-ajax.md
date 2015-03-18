---
layout: post
title: Autosaving with Ajax
category: programming
tags: [redactorjs, ruby-on-rails, javascript, ajax, story-vine]
---

For our final project at [Dev Bootcamp](http://devbootcamp.com), my group and I created a web app called [Story Vine](http://story-vine-app.herokuapp.com/).

<!-- more -->

Quick background story:
>Story Vine is a writing web app that serves a community of writers to share ideas and write stories. It was inspired by the fact that most works of literature have started out as a simple idea, like a seed. This app provides a place for writers to share an idea in the form of a snippet whether it be a dream, a memory, or perhaps a random thought, and watch them come to life through stories.



One of the features we definitely knew we wanted to implement was a [*WYSIWYG*](http://en.wikipedia.org/wiki/WYSIWYG). We wanted to give our users the same look and feel as writing a blog post.

For this, I looked into [Redactor JS](http://imperavi.com/redactor/) and fortunately, there's [a rails gem for it](https://github.com/SammyLin/redactor-rails). Easy peasy, right? Not exactly, unfortunately not all of its features seamlessly transferred over.

One of the features we wanted to use was autosave and since we couldn't get Redactor's built-in autosave to work, I decided to write one on my own using a couple of Ajax calls.

The first thing was to figure out what *type* of Ajax call to make. Personally, I couldn't wrap my head around having the type be `post` and then have it change to `patch` immediately afterward.

So I decided that upon landing on the `new` page, a new snippet should be created immediately so that way the type for the Ajax call could just be a `patch`.

{% highlight ruby %}
# New method in Snippet Controller
  def new
    @snippet = Snippet.create
  end

{% endhighlight %}

However, I quickly realized that there was a problem creating snippets this way. A new snippet is created every time a user lands on this route, and if they were to ever leave the page without typing anything, there would be a lot of nil content saved to the database.

To fix that, I decided that if a user were to ever leave the page via some way that *wasn't* through the submit button, the snippet would be deleted.

{% highlight js %}
var DeleteSnippetWidget = {}


// This method prevents JavaScript errors on pages that do not contain this element.
DeleteSnippetWidget.autoDeleteIncompleteSnippet = function() {

  // Finds an element with the id of 'submit_new_content' and sets it equal to the variable
  var submitNewContentExists = document.getElementById("submit_new_content")

  // Calls LeavePage() if that element exists
  if(submitNewContentExists) {
    this.LeavePage();
  }
}

DeleteSnippetWidget.LeavePage = function() {

  var editSnippetExists = document.getElementsByClassName('edit_snippet');

  // If the button of the form is clicked, set btn_clicked equal to true.
  $('#submit_new_content').on('click', function() {
    window.btn_clicked = true;
  })

  window.onbeforeunload = function(event){
      // If the user leaves the page and btn_clicked is false (not clicked on), use an Ajax call to delete the snippet.
      if(!window.btn_clicked){
        if(editSnippetExists){
            var $url = $('.edit_snippet')[0].action;
            $.ajax({
              type: "DELETE",
              url: $url,
              dataType: "text"
            }).done(function(response){
              console.log("Success");
            });
        }
      }
  }
}
{% endhighlight %}

Now for the actual autosave function.

{% highlight js %}
var SaveWidget = {}

SaveWidget.autoSaveContent = function() {
  var autosaveOnFocus;

  // Finds 'edit_snippet' class element and saves it to a variable
  var editSnippetExists = document.getElementsByClassName('edit_snippet')[0];

  // When the HTML element is on focus, set an interval to call the autosaveSnippet function every two seconds.
  $('.redactor_editor').focus(function() {
    if (editSnippetExists) {
      autosaveOnFocus = setInterval(this.autosaveSnippet, 2000);
  }.bind(this));

  // Clear the interval when the HTML element is no longer in focus.
  $('.redactor_editor').blur(function() {
    clearInterval(autosaveOnFocus);
  })
}

// Ajax call to update the snippet.
SaveWidget.autosaveSnippet = function() {
  var $url = $('.edit_snippet')[0].action;
  var $data = $('.edit_snippet').serialize();
  var $textbox = $('.redactor_editor');
  if($textbox.html().length > 8) {
    $.ajax({
      type: "PATCH",
      url: $url,
      data: $data,
      dataType: "text"
    }).done(function(response){

      // Puts the response into an HTML element with the class of autosave
      $(".autosave").html(response);

    });
  }
}
{% endhighlight %}

I should also add that I removed all of the turbolinks from our rails project in the event that there might be any interference.

One thing that I didn't take into account were users who may leave this page open with the HTML element in focus, possibly resulting in a lot of unnecessary ajax calls. This could probably be fixed with making an Ajax call after a certain amount of key ups events instead of setting an interval when the element is in focus.

Also, in case you're wondering what the update controller method looks like:

{% highlight ruby %}
# Update method in Snippet Controller
  def update
    # Set @snippet to the snippet the user is working on
    @snippet = Snippet.find(params["id"])

    # Sanitize the snippet (this is done using a rails gem)
    snippet_content = Sanitize.fragment(params["snippet"]["content"], Sanitize::Config::BASIC)

    # If the snippet is successfully updated
    if @snippet.update(content: snippet_content)

      # Add it to the current user's snippet collection
      User.find(session[:user_id]).snippets << @snippet

      # If it was an ajax request
      if request.xhr?
        # Render plain and return this text as a response to the ajax call
        render plain: "Autosaved on " + @snippet.updated_at.strftime("%m/%d/%Y at %I:%M:%S %p")
      else
        # Send the user to the snippet's page
        redirect_to snippet_path(@snippet)
      end

    # If the update failed
    else

      # If it was an ajax request
      if request.xhr?
        # Render plain and return this text as a response to the ajax call
        render plain: "Failed to autosave: #{@snippet.errors.messages.inspect}"
      else
        # Send a flash notice to the user and render a new snippet page.
        flash[:notice] = "There was an error saving your snippet. Please make sure it isn't blank."
        render :new
      end
    end
  end
{% endhighlight %}

If you have any other questions, feel free to <a href="http://twitter.com/mxngyn" target="_blank">reach out to me</a>!

**Other References:**

*   [.focus()](http://api.jquery.com/focus/)
*   [.blur()](http://api.jquery.com/blur/)
*   [Ruby on Rails: Using Render](http://guides.rubyonrails.org/layouts_and_rendering.html#using-render)
