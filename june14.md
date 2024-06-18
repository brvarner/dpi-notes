# June 14, 2024 Notes

Today's a remote day, but we're getting into DATA VALIDATIONS! Also, I'm attending the first data structures and algorithms class I've been able to attend

## Data Structure and Algorithms

Today we're tackling array pair sum (aka two sum), which is apparently the most popular problem in leetcode

## Rails Data Validations

So imagine, if you will, we had a scenario like the one that sucked all of our time away in the MSM GUI refactor. It'd be a lot easier if we could ensure every movie had a director ID before they entered our db so that we didn't need to waste time and code validating that info on the client side.

Here's the [official Ruby documentation](https://guides.rubyonrails.org/active_record_validations.html) on data validation methods.

### Validations happen in Models

When creating a model, it's important to make sure certain data is present, unique, or otherwise there for identification

- foreign keys especially should be present in tables so the field won't be useless
- a name, or film title for example, can be validated for uniqueness as well before a record can be created
- Here's a code example:

```
# app/models/movie.rb

class Movie < ApplicationRecord
  validates(:director_id, presence: true)
  validates(:title, uniqueness: true)

  def director
  # ...
end
```

#### Errors

- rails has built-in ways to check while a record didn't save
- `obj.errors` provides a errors object, a collection of validation failures
- ex:

```
[4] pry(main)> m.errors
=> #<ActiveModel::Errors [#<ActiveModel::Error attribute=director_id, type=blank, options={}>]>
```

- `obj.errors.messages` accesses the messages attribute, a hash with a key that's the name of the column with an issue, and a value that's an array of strings describing the problem
- ex:

```
[5] pry(main)> m.errors.messages
=> {:director_id=>["can't be blank"]}
```

- `obj.errors.full_messages` does the same thing as messages, but returns an array of strings in a sentence structure

```
[6] pry(main)> m.errors.full_messages
=> ["Director can't be blank"]

```
