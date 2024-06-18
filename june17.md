# June 17, 2024 Notes

Today, we went over some Microshaft Windblows Word stuff, and Ian's going over some ActiveRecord stuff via must see movies

## Ian's Corner

- Active record pattern predates Ruby, it was a theoretical idea from Martin Fowler before DHH implemented it in Rails
- Summary: **WOW**

### Rails Generate Resource

- When building out your database/models, the "rails generate resource" command with your schema's fields like `name:string year:integer` etc, it'll automatically create view folders, Models, and a controller for the new Model you're creating
- You'll still have to pull the `rake db migrate` action

### Resources

- putting resources, and then the symbol for your Model, it will automatically include the standard CRUD actions for your route
- normally you'd have to type like `get("/movies/", controller: "movies", action: "index")`
- by typing `resources :movies` you not only get an index, but you automatically also have the post routes, the patch routes, etc

### SQLite Viewer

- The SQLite viewer VSCode extension allows you to browse your tables without leaving vscode

### find method

If you want to find a single record in the database, you can use the .find method, which will search directly on the IDs. You can use find_by to search by fields outside of ID

- EX: `Director.find(self.director_id)`

### Belongs to

If you add `belongs_to :director` inside a Model, it automatically knows it's going to look at another table by using a foreign key, in this case, it's in a Movie model with director_id as a foreign key in each row

### includes

If you're querying something that requires data from another table, in your initial query, you can write .includes

- Ex: `Movie.includes(:director).all`
  - If we just queryed movies, we'd have to include a new query every single row when laying out the data, and that'd mean tons of queries. If we used includes, it'd bring in all the directors in a query after getting that data from the movies table, which would be two queries total
