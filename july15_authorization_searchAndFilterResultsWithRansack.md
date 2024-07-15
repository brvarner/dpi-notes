# July 15, 2024 Notes

Today we're reviewing authorization! Ian took us over searching and filtering with the ransack gem.

## Ian's Corner

[Ransack](https://activerecord-hackery.github.io/ransack/) is a gem that assists with searching and filtering values in your project

### Ransack

Ransack has several things, including predicates which define the search for you

- Under the hood, ransack is creating specific active_record requests for your query using the simple syntax you've defined

#### Predicates

These are additions you put on your search query, these can be things like \_cont(contains), \_eq (equals), \_start (starts with), etc

- You can read more on [Predicates here](https://activerecord-hackery.github.io/ransack/getting-started/using-predicates/)

## Auth in Rails

There are a few methods we can use to increase our app's security

### Except

- The `except:` keyword in the router limits the routes the user can access
  - Ex: `resources :follow_requests, except: [:index, :show, :new, :edit, :create]`
    - With this addition to our scaffolded routes, we can remove five of them
    - With this change to our routes file, we break our follow_requests however, as the create action is now off limits.
      - We fix this by removing create from the array: `resources :follow_requests, except: [:index, :show, :new, :edit]`

### Only

- The only keyword allows us to only expose specific routes to our user
- So for likes in a photogram app, the user really only needs access to create and destroy, they really don't need to see everything.
  - ex: `resources :likes, only: [:create, :destroy]`
