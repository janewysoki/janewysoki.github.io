---
layout: post
title:      "Flatiron's Rails Portfolio Project"
date:       2020-04-19 17:22:59 +0000
permalink:  flatirons_rails_portfolio_project
---


My Rails project was more difficult to build than the others, but it was the most rewarding to complete. I struggled to grasp certain concepts throughout the module (through no fault of my instructor’s), but now that I’m working from home, I’m able to dedicate more of my attention to figuring things out. 

One thing that took me a while to understand was nested routes. Because it’s common to have some resources that are children of other resources, nested routes make it possible for us to capture this relationship in our routing. Thankfully, it’s not considered a good idea to have resources nested more than one level deep – it becomes too confusing and makes things difficult. 

My app has a `Book` model with a `has_many` association with the `Review` model, making `Review` the child for `Book`. I nested my routes like so:

```
 resources :books do
    resources :reviews, only: [:new, :index, :show]
  end
```

By doing this, I generated routes such as `/book/:book_id/reviews/review_id` – which is an awesome page to have access to! If I left the resources separate and un-nested, I would never have gained access to that complex route. I’d be left with `/books/:book_id` and `reviews/review_id` instead. Learning the purpose of nested routes, helped me better understand how to use them.

Another thing I learned thanks to working on this project was how to use Materialize to style my app. At times I felt I was making my code less DRY by adding so much HTML to it, but when I saw the results I knew it was worth it. I learned how simple it is to change font color and background color, how to add pictures and place them where you want. Though I’ve completed my project, I’m still having fun browsing Materialize’s website and seeing what else I can do.

