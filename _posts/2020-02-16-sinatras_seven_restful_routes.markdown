---
layout: post
title:      "Sinatra's Seven Restful Routes"
date:       2020-02-16 13:28:14 -0500
permalink:  sinatras_seven_restful_routes
---

Before learning Sinatra, I had never thought about there being a convention for how to handle naming URLs.  I was surprised to learn that there’s a design pattern in place that provides a way for users and developers to easily manipulate data. This convention is referred to as Restful Routes. Restful routes don’t only depend on the URL to know what site to visit, they also rely on an HTTP verb. 

Fully understanding the Restful Routes before you begin your Flatiron Sinatra portfolio project will make your life much easier.  My Sinatra project was a book tracker, so I’ll be using “books” to explain how to use each route. 

1. The first route is `'/books'` and it’s paired with the HTTP verb, GET. This is considered an index action, meaning it’s used to display an index page of all the books in the database. Setting the instance variable `@books` equal to `Book.all` gives the user access to all the books in the database, and also allows you to iterate over `@books` in your books/index.erb file.

2. The second route is `'/books/new'` and it’s paired with the HTTP verb, GET. This is considered a new action and it’s used to display the form a user must fill out in order to add a new book to their list. The form is built in the books/new.erb file.

3. The third route is `'/articles'` and it’s paired with the HTTP verb, POST. This is considered a create action, meaning it creates a new book based on the params from the form you built in your books/new.erb file. The action redirects the user to the book’s show page using `"/books/#{@books.id}"`.

4. The fourth route is `'/articles/:id'` and it’s paired with the HTTP verb, GET. This is considered a show action and it’s used to display one book based on the ID in the URL. It sets the instance variable `@book` equal to `Book.find_by_id(params[:id])`. Because this route uses a dynamic URL (signified by :id), this line of code lets us access the ID of the book through the params hash. It also gives the books/show.erb file access to the `@book` instance variable which is then used to display the book details. 

5. The fifth route is `'/articles/:id/edit'` and it’s paired with the HTTP verb, GET. This is considered an edit action and it will display the edit form based on the ID in the URL. It will send the user to the books/edit.erb file, where the edit form is built. Once again, we set the instance variable `@book` equal to `Book.find_by_id(params[:id])`, which allows us to use `@book` in our code that creates the edit form. 

6. The sixth route is `'/articles/:id'` and it’s paired with the HTTP verb, PATCH. It can also be paired with the HTTP verb, GET. These are considered update action and they handle the submission of the edit form. PATCH will modify an existing book using the information submitted in the edit form, while PUT will replace an existing book. First, we pull the book using the ID from the URL by setting the instance variable `@book` equal to `Book.find_by_id(params[:id])`. Then we use the Active Record method, update (`@book.update`) and pass in a hash of key value pairs. The update method will also take care of saving the new params to the database. The action redirects the user to the book’s show page using `“/books/#{@books.id}”`.

7. The seventh route is `'/articles/:id'` and it’s paired with the HTTP verb, DELETE.  This is considered a delete action and it will delete one book based on the ID in the URL. We once again set the instance variable `@book` equal to `Book.find_by_id(params[:id])` in order to find the book based on the ID in the URL. After finding the book in the database, `@book.delete` will delete the book from the database. This action can then redirect the user back to the index page using `‘/books’`.

