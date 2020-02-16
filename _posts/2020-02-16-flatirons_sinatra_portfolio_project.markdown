---
layout: post
title:      "Flatiron's Sinatra Portfolio Project"
date:       2020-02-16 18:41:07 +0000
permalink:  flatirons_sinatra_portfolio_project
---


The following list contains advice to my past self (and to future Flatiron students) on how to go about completing the Sinatra Portfolio Project. While I don’t cover every minute detail that went into building my application, this list is a general outline that may be used as a reference for those who are overwhelmed or unsure of how to get started. 

1. It’s not absolutely necessary, but planning out your MVC file structure before you begin is a great way to make sure you’re meeting project requirements. I think doing so would have helped provide me with a more thorough understanding of the application I was about to build and just how many steps I had to complete. It would be beneficial for determining how much time to dedicate to your project each day. Use the corneal gem to get your file structure started.

2. Start your actual coding with your models and migrations. They go hand-in-hand and are the easiest place to begin.  Make sure your models inherit from ActiveRecord::Base and that you include your has_many and belongs_to associations. Next, create your migration tables.  Don’t forget to add a user_id/foreign key column to your “child” table.  The value of the foreign key column is the primary key of the “child” table’s “parent”.  Knowing the columns of your tables enables you create a seeds file (in the db folder). Add your seed data here, then run “rake db:seed”. Doing so executes the code in the seed file and inserts it into your database so that you can play around with it.

3. I’d turn to building out your Application Controller class next. This is where I placed my helper methods. Your helper methods are accessible in the views and provide some logical support. I’d build out a current_user method first, followed by a logged_in? method. A user’s ability to view and interact with a web application is often dependent on the user being logged in, so a logged_in? method is a necessity. One place I’d suggest using the logged_in? method is in your layout.erb file to create a nav bar of links that only appear to users who are logged in. I included four links: one that showed the user all books that have been added (by all users), one to add a new book, one to view the books that the user personally added, and one to log out. Another helper method I found immensely useful was creating a redirect_if_not_logged_in method, which does exactly what it says – redirects a user to the welcome page if they are not currently logged in.  I used this method multiple times in both my Users Controller and Books Controller (my application allowed users to keep track of books they’ve read) and it made my code much cleaner. Make sure to “run Application Controller” in config.ru, and to “use” your other controllers there as well.

4. I think creating your Users Controller would be the simplest next step. The purpose of the most important routes in this controller is to render a log-in form, receive the log-in form, render a sign-up form, and create a new user. Having these methods set up makes creating forms in the user login.erb and signup.erb view files the logical next step. 

5. Now that you’ve created the log-in and sign-up forms, it would make sense to build out your welcome.erb page. Here you should include links or buttons that will direct the user to the log-in and sign-up forms you just added.

6. Building my Books Controller was what I found the most challenging. I suggest really studying and understanding the Sinatra Restful Routes before doing so. My blog post prior to this one covers restful routes and might be helpful to review.  This controller will contain an index action, a new action, a create action, a show action, an edit action, an update action, and a delete action (totaling seven routes that follow the Restful Routes convention). Create the corresponding view pages (index, new, show, and edit) as you build out each method. Don’t forget to include “Rack::MethodOverride” in your config.ru file, placed above all controllers. Remember, you need to include a hidden input field to forms that get submitted via patch and delete requests. 

7. The bulk of your application is now complete. Run through the assignment requirements to make sure you meet each one, then you can add flash messages and/or play around with your main.css folder to enhance the user’s experience. 

Good luck!

