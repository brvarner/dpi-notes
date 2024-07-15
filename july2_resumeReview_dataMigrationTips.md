# July 2, 2024 Notes

Today, Ansam is BACK. She's giving us tips on how to revise our resumes. Ian's going over data migrations

## Ian's Corner

Migrations! 🐦

### Create Migrations

- If you're adding a column to a table, try to stick to the convention of "Add X To Y" (ex: AddEmailToUsers)
  - not only is it good practice, but Rails can also sometimes tell what's happening and set up the migration for you
  - in the terminal, it'd look more like `rails g migration AddEmailToUsers email:string`

### Scope vs method

#### Scope

- if you're doing a query to return a value, you can create a scope

#### Method

- If you want to define a transformation that'll happen on a value, you create a method

## Resume Workshop

There are a few tips that Ansam gave that might be good to remember

### Formatting

- If you've got too much text, you can decrease the margin size
- You can **bold** key information to make it stand out if you're afraid if it'll get lost

### Bullet Points - Accomplished [X] by doing [Y] measured in [Z]

- This is the basic format for your bullet points
- Always try to include percentages and numbers
  - If you don't have numbers,either include a positive result, or make up a number or percentage based on your recall of the situation
- Start with an action verb in your bullet points
- Avoid using "I" or "we"
