# June 25, 2024 Notes

Today we're hearing from Kat Jarboe on a day in the life of a software developer/Jira!

## Helper Methods

### link_to

- You can use `link_to` to create a railsified link without using an `<a>` tag in your .html.erb files
  - Ex: `<%= link_to "Go to Google", "https://www.google.com" %>`
    - the pattern is always link_to 'text', 'url'
  - here's an example with the pathing:
  - `<%= link_to 'Add a new movie', new_movie_path %>`

#### No need for Pathing

- You can, in a show details setting, omit the path and just provide a an ActiveRecord. Rails will navigate to it just fine.
  - ex: `<%= link_to "Show details", a_movie %>`
  - What’s happening here? Secretly, link_to checks and says, what `.class` is a_movie? In this case, that would return Movie. Then it does a `.downcase`. Then it does a + `"_url"` on that, and then it calls that helper method:
    - `a_movie.class.downcase + "_url"`
    - `link_to` looks for a named route helper method that matches the name of `a_movie`’s class (`Movie`), because that’s the convention. If you have a table called movies, you’re going to have RESTful routes with names like "/movies" and "/movie/ID". If you’re doing everything by convention, this shorthand works!

#### Deleting with rails link_to

- A delete path should look like this:
  - `<%= link_to "Delete Movie", @movie, data: { turbo_method: :delete } %>`

### form_with

- form_with greatly streamlines ruby form creation
- it also automatically generates the authenticity token step

#### Step 1 - Opening statement

- You start with the opening statement:
  - `<%= form_with(url: path, data: { turbo: false }) do %>`
  - you delineate it with `<% end %>`

#### Step 2 - label_tag

    - You can label your inputs by using rails's built-in `label_tag` functionality
        - ex: `<%= label_tag :title_box, "Title" %>`
        - you start with the input's id, and then enter the title of the input as a string after the comma
            - So in this case, this label is for an input with an id of "title_box", and the innertext is the word "Title"

#### Step 3 - Entering/Submitting Text

##### text_field_tag

- For a standard input with `type="text"`, you want to include a text_field_tag
- Ex: `<%= text_field_tag :query_title, @the_movie.title, { id: "title_box" } %>`
  - The first argument is the name that you'll have for the query as a symbol, it's how the info will register in the params hash
  - Then, you include the value, in this case, it's @the_movie.title
  - Lastly, you can include a hash where you get additional options. in this case, we're adding an id to link the label with the input

##### text_area_tag

- when using a textarea element instead of an input, you must use the text_area_tag
  - ex: `<%= text_area_tag :query_description, @the_movie.description, { id: "description_box", rows: 3 } %>`
    - The same basic principles apply, only difference is specifying the amount of rows in the hash
      - note that rows is an **_integer_** and not a String

##### button_tag

- Lastly, we include the button_tag to submit the form
- Ex: `<%= button_tag "Create Movie" %>`

#### patch

- When creating an update form, be sure to include the `method: :patch` tag in your `form_with` opening
  Ex: `<%= form_with(url: movie_path(@the_movie), method: :patch, data: { turbo: false }) do %>`

### Simplified form

You can drasticallly reduce the text in the form layout with a few shortcuts:

```
  <div>
    <%= label_tag :title %>

    <%= text_field_tag :title, @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag :description, @movie.description, { rows: 3 } %>
  </div>
```

- Above, notice that in the label tag only has one symbol, it'll automatically go to the right text field
- The text field and tag have one symbol laying out the name and the value, and rails defaults to rendering elements with those names as the IDs

### Bundled Sub-hashes in params

- So imagine you have an input with a name in your form (which you need for it to work)
- you can add brackets after the name of your input to store its values in a hash
  - So if you just started from the beginning: `<input name="zebra">`
  - Checking the server, you'll see the params hash (after typing "hi"): `Parameters: {"zebra"=>"hi"}`
  - Adding the brackets: `<input name="zebra[]">`
    - Gives you an array in the hash: `Parameters: {"zebra"=>["hi"]}`
- but adding multiple inputs with that name gives you an interesting variable to map through when receiving user data
  - so two of these:
  ```
  <input name="zebra[]">
  <input name="zebra[]">
  <button>Submit</button>
  ```
  - gives you this params hash: `Parameters: {"zebra"=>["hi", "there"]}`
- you can even give these values their own keys to turn the array into a hash
  - so:
    ```
    <input name="zebra[giraffe]">
    <input name="zebra[elephant]">
    <button>Submit</button>
    ```
  ```
  - gives you: `Parameters: {"zebra"=>{"giraffe"=>"hi", "elephant"=>"there"}}`
  ```
- A nested hash like this is called a **sub-hash**
  - This is the technique that we use more often when we’re building forms. All of the inputs that are related to one model object, a Movie or a Director, are bundled into one sub-hash and stored with the key movie or director. We want that resource to be a top level key in the params hash and then the sub-hash should have all of the attributes (or column values) for that top level key.

#### Bundling Subhash with our revised form

- to get that same subhash with our revised form, we'd have to change the name of inputs
  - So it becomes: `<%= text_field_tag "movie[title]", @movie.title %>`
  - in the controller, the fetch becomes:
    ```
    @movie.title = params.fetch(:movie).fetch(:title)
    @movie.description = params.fetch(:movie).fetch(:description)
    ```

#### Initializing new entries using a hash

- you can fetch information from a form to create a new entry of a Model's thing by requiring and permitting the hash name and subhash values
  - ex:
  ```
   def create
    movie_attributes = params.require(:movie).permit(:title, :description)
    @movie = Movie.new(movie_attributes)
  ```
  - **whitelisting** - this process is called whitelisting columns with **strong Parameters**
    - You need to do this to prevent attacks where people can edit

#### Updating with hash

- When using a hash, you can access and modify things a bit more concisely
- ex:

  ```
      movie = Movie.find(params.fetch(:id))

    movie_attributes = params.require(:movie).permit(:title, :description)

    movie[:title] = movie_attributes[:title]
    movie[:description] = movie_attributes[:description]
  ```

### Form building with model

- In our current state, the label fors don't match our new form layout
- We can change that by using `form_with(model: ...)` instead of `form_with(url: ...)`
  - this layout tells the form we want to create a new movie object

#### Form builder object

- the form object has methods like .label .text_field and more

  - so:
    ```
    <%= label_tag :title %>
    <%= text_field_tag "movie[title]", @movie.title %>
    ```
  - becomes:

  ```
  <%= form_with(model: @movie, data: {turbo: false}) do |form| %>

  <div>
  <%= form.label :title %>
  <%= form.text_field :title %>
  </div>
  ```

  - notice that we've added `do |form|` to the opening erb so that we have access to the form object

- Using the form builder model:

  ```
  <%= form_with(model: @movie, data: {turbo: false}) do |form| %>

  <div>
  <%= form.label :title %>
  <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>
    <%= form.text_area :description, { rows: 3} %>
  </div>

  <%= form.button %>
  <% end %>
  ```

  - We don't even have to name the form button, because rails checks to see if the new object doesn't exist yet, so it knows that it's running a create route on the model

## Routing

- With all of that, we can replace the entire routing around movies in our config/routes.rb with one line: `resources :movies`

## Day in the life of a Software Engineer

- Standups twice a week
- 1:1 with team members
  - great time to get feedback from a supervisor
  - Always try to maximize this time
- Meetings: team meeting, demo day
- Coding and version control
- Reading documentation and learning
  - Reading articles, tutorials, etc,
- Writing documentation
  - keep in mind you're writing it for someone with no context of what you're working on
  - **most of the documentation is in the pull request descriptions**

## Technical Challenges and Tips

- Getting familiar with a new architecture
  - learn how you learn!
  - whiteboard - **excalidraw**
    - lets you draw out every stage of what your code does
- Learning a new programming language independently
  - Use tutorials effectively: code along!
  - ChatGPT
- Reading someone else's code
  - take notes
  - follow the data flow
  - don't lose track of the big picture

## EQ Challenges and Tips

- Building habits and organizing time
- Describing your work
  - Practice and prep
- Imposter syndrome
  - Own your narrative
  - Use your network
  - Remember your wins
