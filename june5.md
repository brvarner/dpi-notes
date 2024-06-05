# June 5, 2024 Notes

Today, we're working on integrating Rails with a database, specifically with PostgreSQL. Matthew Klein is giving a presentation on emotional intelligence.

## Emotional Intelligence

- We talked a lot about speaking about our own emotional experience and the importance of emotional intelligence in building relationships.
- We also practiced coming up with our question formats, there's a little formula, ideally all this takes place in a minute or less, but speak slowly and clearly:
  - Start with your full name and thanking the person for attending the riverside
  - Then transition into your background, really hammering home the ways that your career has informed your outlook and work style
  - Then speak on your experience with DPI and how it's helped you grow as a person
  - Then get into the technical aspect of what you're learning at DPI
  - Then ask your question

## Ian's Corner

Today we explored the Photogram app, and we discussed a few cool things

- We discussed [Counter Cache](https://api.rubyonrails.org/classes/ActiveRecord/CounterCache/ClassMethods.html) in Rails, which does all the work of keeping count of something for you

## Rails and PostgreSQL

- We're working on an app today called my_contact_book, and through that, we're learning how to initialize postgreSQL projects and work with them in Ruby

### Getting Started: Postgres from the Command Line

- Our first step is installing Postgres, which we've already done
- Then we can launch Postgres from the command line with the `psql` command
  - After you launch postgres, your prompt will be replaced with `postgres=#`, indicating that you can start entering postgres commands

#### Postgres Commands

- `\list` - Shows you all the available databases you can access
- `CREATE DATABASE [database_name]` - creates a database and allows you to give it a name, always in snake_case
- `\connect [database_name]` - transitions from a general overview of the database and connects you to a specific database in the list, where you can start performing operations on the data
  - after you connect, your prompt should read `[database_name]=#` to indicate you're working in that database
- `\dt` - Shows you all the tables in your database once you've connected

### Postgres Notes

- **Relation** - a relation is a formal term for a "table". A relation is a set of multiple records

#### Creating a table in your database

- When creating a table, you can initialize it with the fields it should expect. For the `my_address_book` exercise, here's an example initialization:

```
CREATE TABLE contacts (
  id SERIAL PRIMARY KEY,
  first_name TEXT,
  last_name TEXT,
  date_of_birth DATE,
  street_address_1 TEXT,
  street_address_2 TEXT,
  city TEXT,
  state TEXT,
  zip TEXT,
  phone TEXT,
  notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

- **id** will always be SERIAL PRIMARY KEY so it can autoincrement and be marked as your primary key
- **data types** - always include the data types that the values will hold next to the field name

### Queries

- **Grab Data** - the `SELECT` command is how you bring data your way in SQL language.
  - `SELECT * FROM [table_name]` will bring you all values from all fields in a table
- **Add Data** - the `INSERT INTO` command is how you add records to your table - this has a special syntax, where you start by listing the fields, then add their corresponding values in a connected set of parentheses. - Ex:
  ```
  INSERT INTO contacts (
  first_name,
  last_name,
  date_of_birth,
  street_address_1,
  street_address_2,
  city,
  state,
  zip,
  phone,
  notes
  ) VALUES (
  'Carol',
  'Reynolds',
  '2016-10-20',
  '4556 Mirna Shores',
  'Apt. 111',
  'Stromanhaven',
  'DE',
  '13654-8312',
  '308-571-8066 x3565',
  'We had a wonderful dinner at La Tavola!'
  );
  ```

### Connecting Postgres to your Rails project

Here's how we connect Rails to our project and get working

#### config/database.yml

- This file is standard in all Rails projects
- `.yml` files are usually used for configuration and settings in projects
- Among other things, here's where we tell Rails which database we want to connect to
  - **by default, the database setting on line 27 is `rails_template_development`**
- In our project, we would use the database we created, `my_contact_book`

#### rails dbconsole command

- This command in the terminal will launch psql and automatically connect to the table we specify in the `config/database.yml` file

#### rails/db GUI

- Rails provides a GUI that lets you access, import, and edit information to your database
- **Accessing GUI**

  - Start by spinning up your server using the `bin/server` command
  - Once you have your live app preview open, navigate to the `rails/db` endpoint

- **Using GUI**
  - On the left sidebar, you'll see all the tables in the database
  - Once you click a table's name, you'll see the SQL data in that table
  - Click the **+ ADD** button in the top right, and you'll receive a prompt to add more rows via a simple form
  - You can also use the SQL Editor to perform commands in raw SQL

### ActiveRecord

- ActiveRecord is a Ruby gem that automates the process of connecting to a database and performing CRUD operations

#### Models

- Every time we want to interact with a table, we must create a Ruby class that can translate our actions to that table
  - The class:
    - generates SQL
    - sends it to the database
    - transforms the data back into Ruby objects we can use easily
- These models live in the `app/models` folder, you should name the file afer the class it holds
  - By putting it in this folder and naming it properly, every other file in the app will automatically have access to it
- We name the class singularly because each instance of it represents one row in the table
- Rails automatically infers the table name you want to interact with by looking at the class name (e.g., Contact class makes Rails guess there's probably a contacts table you want to be working with)

  - If, for some reason, you give your model a different name for the table **(NOT RECOMMENDED)**, you can still connect it with the `self.table_name = "[table_name]"` line of code

    - Ex:

    ```
    # app/models/zebra.rb

    class Zebra < ActiveRecord::Base
    self.table_name = "contacts"
    end

    ```

    - We'd **NEVER** have two models interacting with the same table, but this is a demonstration that it's possible

##### Inheriting from Active Record

- If we want our models to have the hundreds of methods we need to quickly access for CRUD operations, we should make sure they inherit from ActiveRecord::Base
- EX:

```

# app/models/contact.rb

class Contact < ActiveRecord::Base
end

```

### rails console command

- this command spins up an instance of IRB in the console, basically allowing us to run one line of code at a time for testing
  - This differs as it requires all of our files and gems so we can use stuff immediately
- if you change or add a model, you'll have to restart rails console for changes to affect any dev work you may be doing

### Creating a new record with ActiveRecord in Rails console

Here's the process for adding another contact to the table now that we've created a class that inherits from ActiveRecord

- type `rails c` or `rails console` to launch the rails console
- type `variable_name = Class.new` to create a new row
  - You should see the address of the new row, and all of its values (nil if you initialized without providing info)
  - so for instance if we typed `x = Contact.new`, it'd create a new contact for our `contacts` table and we'd be able to see nils
- Now we can start adding column info for our entry, as ActiveRecord automatically allows us attribute accessors
  - ex: `x.first_name = "Alice"`
- Once we add all the info we want for this row, we can use the `variable_name.save` command, which will run an `INSERT INTO` query
- Here's an example of how it all works together:

```
[1] pry(main)> x = Contact.new
=> #<Contact:0x00007efe0b7e0030 id: nil, first_name: nil, last_name: nil, date_of_birth: nil, street_address_1: nil, street_address_2: nil, city: nil, state: nil, zip: nil, phone: nil, notes: nil, created_at: nil>

[2] pry(main)> x.first_name = "Alice"
=> "Alice"

[3] pry(main)> x.last_name = "Boyer"
=> "Boyer"

[4] pry(main)> x.save
  TRANSACTION (0.6ms)  BEGIN
  Contact Create (13.8ms)  INSERT INTO "contacts" ("first_name", "last_name", "date_of_birth", "street_address_1", "street_address_2", "city", "state", "zip", "phone", "notes", "created_at") VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11) RETURNING "id"  [["first_name", "Alice"], ["last_name", "Boyer"], ["date_of_birth", nil], ["street_address_1", nil], ["street_address_2", nil], ["city", nil], ["state", nil], ["zip", nil], ["phone", nil], ["notes", nil], ["created_at", "2023-07-28 19:17:17.932364"]]
  TRANSACTION (6.4ms)  COMMIT
=> true

```

## Rake Tasks

We can create our own custom scripts to perform specific tasks when we call them, these are called **rake tasks**

- Rails provides a place to put Ruby scripts: `lib/tasks`
- These files have a `.rake` extension, not a `.rb` extension
- Ex:

```
# lib/tasks/i_am_lazy.rake

task(:howdy) do
  pp "Hello!"
end
```

- And in the console:

```
contact-book main % rake howdy
"Hello!"
```
