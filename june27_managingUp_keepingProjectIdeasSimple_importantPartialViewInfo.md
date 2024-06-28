# June 27, 2024 notes

Today, Haneen is teaching us about managing up! Today, Ian is explaining keepnig things simple

## Ian's Corner

- Everything should be made as simple as possible, but not simpler
- The first step towards implementing a full product is eliminating the biggest unknowns with small, experimental, proofs of concept

### The big questions

#### Where is the required info coming from?

- Users?
- API?
- Government CSV?
- Scraping data?

#### Is our domain model correct?

- Did we identify all of the information we need to store to solve the problem
- Is our database architecture correct? - i.e. do we have sufficient tables and columns to store the information

#### Are we solving the right problem?

- Are we solving the right problem, or is an adjacent one more pressing?

##### Who is your user?

- You should be able to point right at a person and say "this is who this is for"
- Can we develop some key personas

### Don't Fall in love with your code, fall in love with your user

- We usually throw away code from 3 or 4 small experiements before we build out anything resembling the first real product

#### Develop and test a hypothesis

- What assumptions are we making about our users?
- How can we test these assumptions quickly?
- Formulate clear hypotheses about your project's key features and validate them
- This approach ensures we **make something people want**
- Fail quickly, and talk to people along the way, don't work on something

### Keep it simple!

- Keep the vision small, and work on something
- What's the smallest thing you can build right now that will allow you to test one hypothesis?

## Managing Up

Managing up is managing your relationship with your manager to develop a productive working rapport

- This task requires empathy to understand the feelings and communication style of your managers and it also requires understanding your own communication style
- **Making your manager's life easier (and inevitably, yours too)**

### Ways to Manage Up

There are many ways to develop your managing up skills

#### Know Your Boss

- **Ask Your Boss**
  - Ask about their preferences in work/communication styles
- **Observe your boss**
  - Is your boss a fast talker? Do they prefer communicating via email, notice what they respond well to
- **Ask other people**
  - Notice who has a great relationship with your boss and ask them about your boss's preference

#### Connect your actions to their goals

- **Be clear about the request**
  - Present your request clearly, add why you want it, and articulate how it benefits you and the org

#### Be a Resource for Help

##### Don't

- Use flattery or sucking up

##### Do

- Look at your boss is, how they operate, and get a clear picture of them and what serves them well
- Take a look at yourself and how you operate, then look at the gap between

##### It's about making a choice

- Managing up means making a choice on how to shift your mindset to adapt and meet your boss's higher needs of priority
- Shift the way you operate in a way that aligns with the way they operate
- **Be in a place of choice** where you're open to trying things differently

#### Help them with their weaknesses

- Bolster their weaknesses by being that support for them
- Have situational and relational awareness to navigate their areas of development

#### Help them know your strengths

- have a conversation with your boss
- be aware of opportunities
- SPEAK UP, ensure they know your qualifications and how you can help

#### Find ways to insert yourself

- Ask your manager if it would be helpful for you to take on specific tasks
- Take the initiative to ask your manager what they prefer in terms of meeting cadence and communication
- Making your manager come up with stuff to do is a terrible way to get them to appreciate your efforts, instead, bring in ideas and try to get things working

### Request. Don't complain

- Inside every complaint is a request. Find it, and make it
  - If your manager often complains about something, decide if you can fix it, then determine how you can fix it, then

## Helper Methods 3

In this project, we're implementing bootstrap styling with the helper methods we worked on with the June 25th assignment

- **Important** - git add -A stages all changes, no more caveman copying

### Bootstrap Navbar

- So with the `link_to` method, our navbar brand looks a bit different
  - It goes from `<a class="navbar-brand" href="/">Helper Methods 3</a>`
  - To: `<%= link_to "Helper Methods 3", root_path, class: "navbar-brand" %>`
  - We can pass additional arguments as a hash on the end of the method, here we state that the class of the link is `navbar-brand` as it was in the original
  - Note the use of `root_path`, which we define in our router/rails knows automatically

### Partial View Templates

- You know how react has components? This is FINALLY how we do that in Rails
- also called partials
- Here's the [official API entry](https://edgeapi.rubyonrails.org/classes/ActionView/PartialRenderer.html) from Rails on partials

#### Building and using your partials

- Partials **must** have an underscore at the beginning of their file names
- so you create your partials in the files, then you render them by linking in the specified view with this syntax: `<%= render partial: "folder/file" %>`
  - so if we created a zebra folder in views with a file called \_giraffe.html.erb inside, that render statement looks like this: `<%= render partial: "zebra/giraffe" %>`
- often, your partials live in a folder called `shared`
- the big difference b/t partials and react components is that you don't have to return something to be rendered, they can handle conditionals or just posting one thing at a time, or importing tags for the head, whatever you think could be removed to make your code more readable, you can put it in its own partial

#### Partials with inputs/passing arguments with locals

##### locals

- the `:locals` option allows us to pass arguments to the partials
- so lets say we set up a partial that greets our visitor: `<h1>Hello, <%= person %>!</h1>`
- If we just drop this partial in, we'll get some errors, because the person isn't defined yet.
  - but we can use a local to make it work a little better
  - so we add the local to feed it something ahead of time: `<%= render partial: 'zebra/elephant', locals: { person: "Alice" } %>`
- the locals take the form of key/value pairs
- so if we're creating or modifying a value, we need to provide it as a local
  - to wit: `<%= render partial: "movies/form", locals: { movie: @movie } %>`

##### concision

- we can make this even shorter, as rails can understand a partial in a folder and that something coming after a comma is a locals
- ex: `<%= render "movies/form", movie: @movie %>`

#### ActiveRecord object partials/to_partial_path

- You can add a partial path to your data Model to shorten your rendered partials even more.
- If you're making a card or something to display a specific item, you wouldn't call it like `_movie_card` by default, just `_movie`

  - So if we wanted to use the movie card partial often to display the Movie data model, we can add the path to its partial like so:
    ```ruby
    class Movie < ApplicationRecord
    validates :title, presence: true
    def to_partial_path
    "movies/movie"
    end
    end
    ```
  - that partial path points to an HTML partial that's a bootstrap card to display movies that's already set up with places for all their info
  - And whenever we're handling the data, we can just say `<%= render @movies %>` to map through a list of Movie objects, or `<%= render @movie %>` to show a single Movie object
    - Rails automatically knows to go to the partial path by the structure of this render statement, and it knows from the to_partial_path method that it's going to be injecting data into a partial for display.

### before_action

this method goes into your controller and lets your controller know it must run a specific method before running other methods. It takes a first argument with a method name as a symbol

#### only

- The second argument in a before_action can be an only
  - The only defines which actions we need to run a certain action before
  - ex: `before_action :foo, only: [:new, :index]`
    - In this example, we've defined a method called foo, which we're asking our controller to run before running the new and index methods

#### except

- except is the same basic principle as only, except it denotes that there are only a few methods that your before_action SHOULDN'T be running before

#### IMPORTANT: USER AUTHENTICATION

- to force users to log in before seeing your site's content, add `before_action :authenticate_user!` to your site's Application Controller

### private keyword

- We can define methods that are helpers for the actions below private in the model's controller
- For example, we've used this line multiple times in our file: `movie_params = params.require(:movie).permit(:title, :description, :image_url)`
  - this line helps both the create and update methods accomplish their jobs
- if we made a private method that handled that logic instead, we could just place movie_params in methods that need them
- Ex:

```
private

def movie_params
params.require(:movie).permit(:title, :description, :image_url)
end
```

- so instead of defining that variable in each action, this method lets people define it once and just plug in movie_params as a function argument when needed

- we can also declare a method to find a specific movie before displaying it, so we don't have to type `@movie = Movie.find(params.fetch(:id))` every time we need to select a specific movie
  - just define a set_movie private method with that same text, and set it as a before_action to get the movie before navigating to pages that need it like show, edit, update, and destroy
