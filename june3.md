# June 3rd, 2024 Notes

Today, we're discussing the unspoken rules of corporate America!

## Unspoken Rules

Morgan is delivering the presentation today. According to Morgan, there's a great resource by Gorick Ng, a former guidance counselor/career coach at Harvard University.

- Many students would come from different cultures, and wouldn't understand the subtleties of American Corporate Culture.
- To remedy this issue he wrote a book called The Unspoken Rules
- Wednesday at 8 a.m., Ruben has a book club, and they're finishing up The Little Black Book of Corporate America.

### Microsoft Office

- One of the unspoken rules of corporate America is that people all know how to use Microsoft Office Suite off the rip, which may not be the case.
- Today we're starting out by taking a Microsoft Office Assessment to identify areas of strength and development
  - We'll cover:
    - Excel
      - Before Juneteenth, a DPI staff member will give a presentation on V Lookups and Pivot Tables
    - Powerpoint
      - Jackie from our UI/UX team will come in and give a presentation on how to make a professional powerpoint presentation
    - Word
    - Outlook

# Ian's Corner

Today, we continued our dive into the relational database swamp with a presentation on Records and Relationships.

## Records and Relationships

- We can create a mental model on how associations work:
  - Cities and Neighborhoods (can one city have many neighborhoods? can one neighborhood have many cities?)
  - Courses and Students
  - Photographers and Photos

### Relationships

- If a relationship is one to many? Which table gets the foreign key column?
- If many-to-many, what's a good name for the join table?
- We created another spreadsheet today, similar to the one we did a few days ago for the movie database.
  - We looked at a todo list and had to decide how to model it.
    - Each task had three stages:
      - Not Yet Started
      - In Progress
      - Completed
    - I initially suggested doing a many-to-many table to keep track of each user's tasks, but Ricardo rightfully noted that it'd be overkill for an app with one user
    - Instead, ended up making a users and a tasks table
      - each user entry has an id, an email, and a password
      - each task entry has its own id, task text, a status marker, and a user id to tie it to its owner

# Getting Started with Rails

We made it! What is happening!?

- OK, so there are a few differences bt Sinatra and Rails, but here's how we get started.
- There are a few key files/concepts we need to know/follow to begin.
  - **config/routes.rb** - This is our app's router
    - Instead of getting our routes by typing: `get ("/") do end`, we now have to type: `get("/rock", {:controller => "", :action => ""})`
      - This structure allows us to break up our app and import different files to perform tasks
      - **controller** - A controller is a class that provides methods which the page runs as it loads
        - controllers live at `app/controllers`
        - in the example above, we have a file called `zebra_controller.rb` that holds a class called `ZebraController` which holds a method called `giraffe`
          - We set this controller up to inherit from its parent class, the ApplicationController (`class ZebraController < ApplicationController`)
          - We're going to be inheriting attributes from parent classes a lot in Rails, taking pre-written methods that we don't need to redefine
            - One of the methods is **render**, which shows something on a certain page.
              - so if we wanted to render plaintext on the page above, we'd type:
                ```
                  class ZebraController < ApplicationController
                    def giraffe
                      render({ :plain => "howdy" })
                    end
                  end
                ```
                - and this code would render the word howdy on the page when we went to the /rock domain.
              - render is more flexible than sinatra's ERB method
                - we're going to use it for JSON later
                - we provide the `:template` key, and then point it towards an html.erb file
                - ex: render({:template => "game_templates/play_rock"})
                  - we don't have to add .html.erb to the end of the filename when rendering with template as of Rails 7.
                - In Rails, you can respond out of the box to many formats, including **JSON, CSV, and more**

## Form Params

- When using form params, you access them in the controller by using **params.fetch()**
  - for example: `@target_num = params.fetch('number').to_f`
