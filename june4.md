# June 4th, 2024 Notes

Today, we're discussing networking with Ruben!

## Networking

We'll probably have two networking sessions according to Ruben.

- It's what you know, but WHO you know, but really it's WHO KNOWS YOU!
- How do we make networking more fun and less intimidating?
  - Today's more about breaking the ice and getting comfortable, maybe Thursday we'll talk about building relationships
  - Whenever going to a networking event, it's always good to have a plan
    - Are you going to meet the speaker?
    - Are you interested in the topic?
    - Do you know if it'll help you?
  - Know who you want to meet?
    - Quantity or quality? **Both!**
      - Sometimes to get to the quality, you have to meet a whole lot of people before you find the ones who are right for you.
  - Naturally, do your research before attending events
    - Ruben updates his list of networking events often, **you can email him to join his list**
      - the one's he's attending are highlighted in red
    - Some organizations will give you a list of attendees before attending events so you can go see who they are on linkedin
      - At the very least, research the speakers
- How to do it
  - You get to the event, you've done the research, but maybe the people that you researched don't show up

### Talk to as many people as possible!

- Network anywhere and everywhere, if you're at an event, anything is an opportunity to meet new folks
- Find out what each person does, and if you know who they are, then you can determine what you think
- Be an active listener, try to get people to talk more about themselves, and make sure to give them your undivided attention
  - Find out their job title
- **Research DPI and know enough to explain it properly**
  - Started when Chicago didn't get Amazon, and they wanted to train a workforce to become a leading tech city.
- Pay attention to body language

#### Breaking into an existing conversation

- If people are already talking, you don't have to just sit there and wait
- Bring a notebook to events, write down people's names and make little notes, especially if it's a high-value connection
- **Ask if you can join people!**
  - Everyone's there to meet people anyway, so there are very low chances that you'll end up being rejected
- You can get people's info at **any time** sometimes it helps to do it at the beginning of the conversation
  - There are lots ways to do it with a digital card.
    - LinkedIn has a built-in QR code that allows people to scan and connect immediately
    - **Take a picture of paper business cards so that you don't lose them**

#### Take the initiative

- If there's a table that's already talking, try and find a connection, and ask if you can have a seat
- **Make Jokes**
  - Most folks are kinda the same
  - C-suite folks especially don't get a lot of folks joking with em, so don't get intimidated
  - **Don't get intimidated!**
    - It's a networking event, EVERYONE is trying to meet people!
    - If you are sitting in a lobby or something, and you're wondering who someone is, go up and talk to them
- How do we maintain a connection?
  - If you meet someone important, you can reach out ASAP
  - Sometimes folks will reach out to you
  - **At MOST, wait 24 hours for a hot lead**
  - **Don't sell people right away, ALWAYS make a connection**

### LinkedIn

- You can reach out to just about anybody as long as you have a good opener
- LinkedIn is your lil **infomercial**, so make sure your profile matches what you're trying to put out there
  - Carefully consider your banner photo. It should reflect who you are and what you're trying to do
- Look at upcoming conferences, and articles in the space you're connecting with someone around to look a bit more connected
  - Ex: Hey, did you see this article? Hey did you know about this thing coming up?
    - Hyperlocal news is great especially, **always bring something to the table**
- LinkedIn has a score for everyone, which ranks you based on your activity and engagement on the platform, it's called an SSI

### Networking Pt. 2 Following up on Networking Events (June 6)

- Never Eat Alone is a good book by Keith
- How to win friends and influence people by Dale Carnegie
- Should I be reading books on Networking?
- Both of these books mention having a **generosity mindset**
  - going into events, thinking about what I can give without expecting anything in return
- 1871 is in Merchandise Mart, it's a startup accelerator/incubator
  - Universities, startups, founders, etc have spaces there
  - They have an event on June 20 for Blk tech makers (BLKtech Pitch Competition)[https://1871.com/event/blktech-pitch-competition/]
- NSBI - National society of Black Engineers
- mHub is an accelerator for physical, manufactured startup items

#### Becoming a part of an organization

- If there's an org that has a lot of what you need, get involved with them, volunteer, show up consistently, get to know people

#### Setting up coffee meetings

- Do your research, reach out to people
- If they meet up with you for coffee, do your research, ask questions, make sure you casually get across your experience, be sure to go outside of work topics to make things more casual

# Ruby Notes

Today, we're learning about Nesting Routes in Rails apps!

## Nested Routes

- Nesting is a way of organizing and structuring code to reflect the hierarchical relationships between different resources
  - For example: if you have a project model with many tasks, you can nest the routes for tasks within the routes for those projects

### When To Use Nested Routes?

- **Hierarchical Data** - When one resource logically belongs to another (e.g. tasks belonging to a project, or a property company has many different real estate listings to show)
- **Scoping** - to scope related resources under their parent resource
- **Clear URLs** - to generate URLs that reflect the relationships between resources (e.g., `projects/:project_id/tasks`)

### Example

```
# config/routes.rb
Rails.application.routes.draw do
  resources :projects do
    resources :tasks
  end
end
```

- On this example, you can create URLs like those above and show which task belongs to which projects

- Here's an example of the RCAV (Render, Controller, Action, View) model in action with Nested Routes:

```
# app/controllers/tasks_controller.rb
class TasksController < ApplicationController
  before_action :set_project

  def index
    @tasks = @project.tasks
  end

  def show
    @task = @project.tasks.find(params[:id])
  end

  def new
    @task = @project.tasks.build
  end

  def create
    @task = @project.tasks.build(task_params)
    if @task.save
      redirect_to [@project, @task], notice: 'Task was successfully created.'
    else
      render :new
    end
  end

  def edit
    @task = @project.tasks.find(params[:id])
  end

  def update
    @task = @project.tasks.find(params[:id])
    if @task.update(task_params)
      redirect_to [@project, @task], notice: 'Task was successfully updated.'
    else
      render :edit
    end
  end

  def destroy
    @task = @project.tasks.find(params[:id])
    @task.destroy
    redirect_to project_tasks_path(@project), notice: 'Task was successfully destroyed.'
  end

  private

  def set_project
    @project = Project.find(params[:project_id])
  end

  def task_params
    params.require(:task).permit(:name, :description, :due_date)
  end
end
```

- Notice a few things about this example:
  - the index method brings all tasks associated with a project
  - you can have public and private methods in Ruby, private methods are inaccessible by users, but can operate within the class

### Nested Attributes

- Nested attributes let you save attributes on associated records via the parent record
  - This tech is useful when dealing with forms that need to update multiple models

#### When to Use Nested Attributes

- **Complex forms** - forms that handle attributes for multiple related models
- **Efficient updates** - updating parent and child records with a single request
- **Caveats**
  - **Limit nesting depth** - Avoid deep nesting, typically you just need one level of nesting
  - **Only use nesting when absolutely necessary**

##### Examples

- The following is how you set up nested attributes in a Rails application

```
# app/models/project.rb
class Project < ApplicationRecord
  has_many :tasks, dependent: :destroy
  accepts_nested_attributes_for :tasks, allow_destroy: true
end
```

- We're telling the app that this class will have several tasks, and it accepts nested attributes for those tasks
- With that set up, you can manage Task records using the Project model. Here's how a possible ProjectsController would look:

```
# app/controllers/projects_controller.rb
class ProjectsController < ApplicationController
  def new
    @project = Project.new
    @project.tasks.build
  end

  def create
    @project = Project.new(project_params)
    if @project.save
      redirect_to @project, notice: 'Project was successfully created.'
    else
      render :new
    end
  end

  def edit
    @project = Project.find(params[:id])
  end

  def update
    @project = Project.find(params[:id])
    if @project.update(project_params)
      redirect_to @project, notice: 'Project was successfully updated.'
    else
      render :edit
    end
  end

  private

  def project_params
    params.require(:project).permit(:name, :description, tasks_attributes: [:id, :name, :description, :due_date, :_destroy])
  end
end
```

- So the last method, project_params, shows that navigation **requires** a project param, and that the permitted flags include everything in the function args
- The rest of the functions are pure CRUD

- Not to be outdone, here's an example form with nested attributes

```
<!-- app/views/projects/_form.html.erb -->
<%= form_with(model: @project) do |form| %>
  <div class="field">
    <%= form.label :name %>
    <%= form.text_field :name %>
  </div>

  <div class="field">
    <%= form.label :description %>
    <%= form.text_area :description %>
  </div>

  <h3>Tasks</h3>
  <%= form.fields_for :tasks do |task_form| %>
    <div class="nested-fields">
      <div class="field">
        <%= task_form.label :name %>
        <%= task_form.text_field :name %>
      </div>
      <div class="field">
        <%= task_form.label :description %>
        <%= task_form.text_area :description %>
      </div>
      <div class="field">
        <%= task_form.label :due_date %>
        <%= task_form.date_field :due_date %>
      </div>
      <%= task_form.check_box :_destroy %>
      <%= task_form.label :_destroy, "Remove task" %>
    </div>
  <% end %>

  <div class="actions">
    <%= form.submit %>
  </div>
<% end %>
```

# Ian's Corner

Today we're talkin BOOTSTRAP

- Bootstrap is a reach goal to add for our future projects, it's good to get some practice

## Bootstrap

- Bootstrap makes it very easy to spin up well designed stuff without spending a lot of time writing CSS rules
- If you want to make your own custom bootstrap styling, you can create it at **[Bootstrap Build](https://bootstrap.build/app)**
- Minified Stylesheets can drastically increase performance

### Breakpoints

- A medium breakpoint will style things differently once you get bigger than maybe a phone or something
- So like setting a medium breakpoint means the app looks diff on small screens

### Browsers view CSS differently

- Different browsers view CSS rules differently
  - webkit is Safari
  - moz is Mozilla
  - Keep rules for multiple browsers to ensure compatibility
