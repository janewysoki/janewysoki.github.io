---
layout: post
title:      "Authentication in Rails"
date:       2020-04-27 18:59:28 +0000
permalink:  authentication_in_rails
---


Most applications today require a user to sign up. While the user is able to sign up, log in and log out, the creator of the application doesn’t want the user to have access to every single part of it (such as raw data, security information and/or customer details). Thankfully, there are multiple ways to set up user authentication in Rails.

There are 4 key authentication and authorization terms that are important to understand. They are abstract and don’t always apply to a specific framework, but it’s helpful to know their meaning.

1. **Identification**: Obtaining an identity claim. For example, a username or email from the user.
2. **Authentication**: Verifying the identity claim. For example, entering a password.
3. **Access Policy**: The given roles a user is allowed to do. For example, granting the user certain privileges based on their identity. 
4. **Authorization**: Checks throughout the application that enforce the Access Policy. For example, not allowing the user to alter specific parts of the site.

A key part of authorization is the management of passwords. Rails provides us with the tools we need to securely store passwords and protect them from hackers. Since we can’t store passwords, we store their hashes. A hash is a number computed by feeding a string to a hash function. Rails then saves the password hashes in the `password_digest `column in the database. Almost all hash functions are designed so that strings that are similar, hash to very different values. 

Our `password_digest `column mainly stores two values: the salt and the actual return value of BCrypt. Rails often uses BCrypt to create and store hashes. BCrypt makes what is a called a “cryptographic hash.” This means that if you have an output in mind, finding a string to produce that output is designed to be so difficult that “even if Google put all their computers on it, they couldn’t do it.” Additionally, BCrypt is designed to work slowly - this means that even if a hacker has a list of passwords to check against, it will still take them a long time to check for the user’s password against that list. These two features make BCrypt a great gem to use for managing passwords. Another way we keep passwords safe is through “Salting”. A salt is a random string prepended to the password before it gets hashed. It’s stored in plain text, so it’s not a secret, but it being there makes a hacker’s job far more difficult. 

Adding the BCrypt gem to your Gemfile provides the developer with a method called `has_secure_password` that can be used on ActiveRecord models to handle the creation of hashes and salts for you. `has_secure_password` adds a `password` field and a `password_confirmation` field to your model. These fields don’t correspond with any database columns because the `has_secure_password` method expects to find the `password_digest` column described above defined in your migrations. `has_secure_password `also adds `before_save` hooks to your model. This compares the `password` and `password_confirmation` fields. If the fields match, then the `password_digest` column gets updated. Bcrypt and `has_secure_password` also enable the `authenticate` method which returns self (typically user) if the password is correct, and false if it is not.

It’s important to note that Rails secures your passwords when they’re stored in the database, but does not secure your servers (which contain passwords in plain text). Additionally, handling password changes and password recovery emails can become tedious and burdensome. Sometimes creating an account with a password is a deterrent to users who just want quick access to an application. All these problems can be solved with OmniAuth - a way to log in to an application through a third-party account (i.e. Google, Facebook, GitHub, etc.). Allowing a user to login through a third-party means the third-party is now responsible for securing passwords and dealing with things like email verification.

OmniAuth is a Rails gem that can be used alongside the more traditional username/password setup. It allows us to use the OAuth protocol with a variety of providers. When adding the `omniauth` gem, you must also add the provider-specific OmniAuth gem (i.e. `omniauth-google`) to your Gemfile. Then you have to tell OmniAuth about your app’s OAuth credentials in the config/initializers folder. 

```
Rails.application.config.middleware.use OmniAuth::Builder do
      provider :facebook, ENV[‘GOOGLE_KEY'], ENV[‘GOOGLE_SECRET']
end
```

`ENV` is a constant that refers to a global hash for your entire computer environment and it stores any key-value pair, making it a useful place to keep credentials. The code in this file tells our Rails app to use a piece of middleware created by OmniAuth for the third-party authentication strategy. It also requires two pieces of information from the third-party in order to get authentication working: the application key and secret. 

Instead of setting environment variables directly in our local `ENV` hash above, we can use the `dotenv-rails` gem. This allows us to ensure that the application key and secret are correctly loaded into the `ENV` hash in a secure manner. This requires the creation of a `.env` file where you load your third-party app credentials, and the addition of `.env `to your `.gitignore` file to ensure you don’t accidentally commit these credentials.

For OAuth to work, the developer also needs to add a route for the third-party to redirect users to in the callback phase of the login process (i.e. `get ‘/auth/google/callback’ => ‘sessions#create’`). We’ll also need a link that will initiate the OAuth process (i.e. `<%= link_to(‘Log in with Google’, ‘/auth/google’) %>`). The code that will get added to our SessionsController is simple and includes a helper method to keep our code dry. We add the following to our create action:

```
def create
        if params[:provider] == 'google_oauth2'
            @user = User.create_by_google_omniauth(auth)
            session[:user_id] = @user.id
	    redirect_to user_path(@user)
	end
end
```

And the following private helper method:

```
def auth
        request.env['omniauth.auth']
end
```

Implementing the OAuth protocol can be difficult, but it’s worth it as it streamlines the authorization process and allows users to easily log in to your application.

The final authentication method I learned in Rails was Devise, which takes care of OmniAuth for you and also handles authentication with BCrypt, eliminating the need to hash and salt passwords manually. Devise allows you to have multiple models signed in at the same time and is based on a modularity concept of “use only what you need.” It hashes and stores a password in the database to validate the authenticity of a user while signing in. Devise is considered to be a cornerstone gem for Rails authentication. Devise uses 10 modules to configure user authentication:
1. **Database Authenticatable**: Hashes and stores the password in the database.
2. **Omniauthable**: Adds support for OmniAuth provider.
3. **Confirmable**: Disables access to the user account unless user has confirmed account through email.
4. **Recoverable**: Includes a “Forgot my Password” link that lets the user reset their password through email.
5. **Registerable**: Creates a registration process so users can edit and delete their account.
6. **Rememberable**: Creates a token and stores a user session with a saved cookie. Also includes the helpful “Remember Me” checkbox upon logging in.
7. **Trackable**: Tracks user IP addresses, sign in count, last sign in and timestamps.
8. **Timeoutable**: Logs a user out after a certain amount of time.
9. **Validatable**: Uses built-in Devise validations for email address and password.
10. **Lockable**: Locks an account after a specific amount of time or after a certain amount of log in attempts.

Six of the modules above (#s 1, 4, 5, 6, 7, 9) are enabled by default, so it’s easy to see how a gem that includes all these functions would be desirable for developers to use in their applications. Devise also comes with helper functions (some of which I created on my own in my Sinatra app and would have been very useful to know about). For example, `before_action :authenticate_user!` can be added to any controller to limit access to an action unless a user is logged in. Unlike OmniAuth, you do not need to set up individual routes to use Devise, but you must add one of the following lines to your routes file:

```
devise_for :users
devise_for :MODELNAME (if not using User as the name of the model)
```

For anyone looking to include Devise in their own application, I suggest following the steps included here: https://github.com/heartcombo/devise.

