---
layout: post
title:      "My Children's Books Ruby CLI Gem"
date:       2019-09-15 18:53:26 -0400
permalink:  my_childrens_books_ruby_cli_gem
---


My first main project at Flatiron was to create a Ruby CLI Gem. 

I began by finding a website that wouldn't be too hard to scrape.  It took a while, but I eventually found Common Sense Media's list of ["50 Books All Kids Should Read Before They're 12"](https://www.commonsensemedia.org/lists/50-books-all-kids-should-read-before-theyre-12).  To start, I created my scraper file and added the following:

```
module ChildrensBooks
    class Scraper
        def self.scrape
            html = open("https://www.commonsensemedia.org/lists/50-books-all-kids-should-read-before-theyre-12")
            doc = Nokogiri::HTML(html)
			  end
	 end
end
```

I knew to set up my `doc` variable because we learned that Nokogiri's .css method gets called on a `doc` variable that's set to equal a string of HTML. Nokogiri is a Ruby gem that allows us to parse HTML and collect data from it. With this step complete, I turned to my website, inspected the page, and used the select tool to hover over the pieces of information I wanted to scrape. In doing so, I learned that my page's HTML had two sets of nested nodes I'd need to iterate over since the list of all 50 books was divided into odd and even number categories. A nested node is any tree of elements in which parent elements branch off to contain children elements. Now that I had my nested nodes to iterate over, I added them to my scraper file, along with the information I planned to scrape:

```
doc.css(".views-row-odd").each do |node|
                title = 
                author = 
                description = 
                age = 
                year = 
                url = 
end

doc.css(".views-row-even").each do |node|
                title = 
                author = 
                description = 
                age = 
                year = 
                url = 
end
```

I proceeded to scrape the information needed to fill my code above, and used methods like `.text`, `.gsub()`, and `.strip` to format the data the way I wanted it to appear to the user.

Next, I created the Book class file because I knew the attributes I'd be using based on my Scraper class. I started by adding my attr_accessors, an empty @@all array, my initialize method, and a self.all class method that would provide an array of all my book object instances. My file looked like this:

```
module ChildrensBooks
    class Book
        attr_accessor :title, :author, :description, :age, :year, :url
        @@all = []

        def initialize(title, author, description, age, year, url)
            @title = title
            @author = author
            @description = description
            @age = age
            @year = year
            @url = url
            @@all << self
        end

        def self.all
            @@all
        end
    end
end
```

An attr_accessor is a macro, a piece of code that writes code for you. Each attr_accessor provides access to a getter and a setter method (aka an attr_reader and attr_writer). Creating attr_accessors allowed me to abstract away the manual, explicit definition of the setter and getter methods, saving me time and keeping my code DRY. In my program, the initialize method takes in the attr_acessors as arguments, which get passed in when instantiating a new Book object, thereby providing each new object with initial data. All newly created Books then get pushed into the @@all array, so that I can later iterate over them. 

Next, I focused my attention on filling out my CLI class file. I began with a simple outline of my call method so that I could visualize the basics of how I wanted my CLI to look and act:

```
module ChildrensBooks
    class CLI

        def call 
            ChildrensBooks::Scraper.scrape
            puts "Welcome to the Children's Books Database!"
            @input = nil
            menu
            while @input != "exit" 
                @input = gets.chomp
                if @input == "1"
                    puts "Here are some books 2-4 year olds will love:"
                elsif @input == "2"
                    puts "Here are some books 5-7 year olds will love:"
                elsif @input == "3"
                    puts "Here are some books 8-9 year olds will love:"
                elsif @input == "4"
                    puts "Here are some books 10-12 year olds will love:"
                elsif @input == "5"
                    puts "Here's a randomly selected book for you to try:"
                elsif @input == "menu"
                    menu
                elsif @input == "exit"
                    break
                else 
                    puts "Sorry, you did not select a valid number. Please try again."
                    menu
                end
            end
            puts "Thanks for visiting. I hope you're headed out to the bookstore!"
        end
    end
end
```

Since I planned to call `menu` at certain points in my call method, I knew I had to create a menu method next:

```
def menu
            puts "Select an age range to see the best children's book suggestions!"
            puts <<-MENU
            1. Ages 2-4
            2. Ages 5-7
            3. Ages 8-9
            4. Ages 10-12
            5. Surprise me!
            Type "exit" to leave the program.
            Type "menu" at any time to return to menu.
            MENU
end
```

The lines of code that say `puts <<-MENU` and `MENU` are called a heredoc. Before I added this, the indentation and formatting of my menu method looked sloppy. A heredoc behaves like a double-quoted string, but without the double-quotes, and it's used to define a multiline string while still maintaining proper indentation and formatting. It was the perfect way to make my menu easy to read. 

Next came the hard part: creating class methods in my Book class that could be used in my CLI class file. Since my menu split the data I scraped into age ranges, I knew I needed to iterate over my @@all array to pull out instances of my Book class based on their age attribute. So, I created the following method that returned all book instances for children ages 2-4:

```
def self.preschoolbooks
            self.all.select do |book|
                book if (2..4).to_a.include?(book.age.to_i)
            end
end
```

I recreated this method 3 more times, changing the method name, and the age range. Next, I needed to figure out how to use these methods in my CLI class. The class methods I'd created only returned instances of books, which meant I had to write instance methods in my Book class that allowed this data to be printed in a legible way for the user. To do so, I wrote the following method:

```
def print_books(array)
            array.each do |book|
                puts "Title: #{book.title}"
                puts "Author: #{book.author}"
                puts "Description: #{book.description}"
                puts "Release Year: #{book.year}"
                puts "Age: #{book.age}"
            end
end
```

This method accepted an argument of an array and iterated over it to print out the information I wanted my user to see. Now I was ready to execute this method in the while loop/if statement section of my CLI class call method. In order for the above method to work, I needed to create an array for it to accept as an argument. I created a `books_array` variable and set it equal to `ChildrensBooks::Books.preschoolbooks`.  Then I entered my `print_books` method and passed in `books_array` as an argument. This allowed my `print_books` method to iterate over the `books_array` (which contained all instances of books for preschool-aged children) and print out the data I wanted to see. Now, my first if statement in my call method was complete:

```
if @input == "1"
           puts "Here are some books 2-4 year olds will love:"
           books_array = ChildrensBooks::Book.preschoolbooks
           print_books(books_array)
```

I filled out the remainder of my call method, and my program was nearly complete. After a few additional changes, I pushed my program to git hub and published my gem. While creating a CLI gem took a lot of time and effort, I was pleasantly surprised by how much I knew how to do on my own. This project made me realize that I've learned (and retained) way more information in a two-month period than I ever thought to be possible. I feel proud of what I've created and more confident in my coding skill-set.
