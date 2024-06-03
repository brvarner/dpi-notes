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
