---
layout: post                   
title: Creating multiple todo-lists in Rails
category: normal
---

// 2017-03-08
Steps I followed:
Controller is like views in django 

https://www.youtube.com/watch?v=fd1Vn-Wvy2w
#### Creating new project:
> rails new todo-rails 

#### Scaffold 
> rails g scaffold todo_list title:string desc:text
> rake db:migrate 
(then go to http://localhost:3000/todo_lists)

#### Change root route to show lists 
> In routes.rb: root "todo_lists#index"

#### Generate a model for our todo items
> rails g model todo_item content:string todo_list:references (to ref the todolist) 
> rake db:migrate 

#### Want to 'and' association btwn todoitem & todolist 
> In todo_list.rb: has_many: todo_items

#### Want to create routes for our todo items 
1. In routes.rb:
```
resources :todo_lists do 
resources :todo_items
end
```

2. Do
> rake routes 

output: We only have a todo item only if a list exists (which makes sense)
```
POST   /todo_lists/:todo_list_id/todo_items(.:format)          todo_items#create
```

#### Generate controller for our todo items
> rails g controller todo_items

```
class TodoItemsController < ApplicationController
	before_action :set_todo_list

	# After we create a todoitem under a todolist 
	# want to redirect to the todo_list path 
	def create 
		@todo_item = @todo_list.todo_items.create(todo_item_params)
		redirect_to @todo_list
	end

	# create private method. TodoList references the model 
	private 

	def set_todo_list
		@todo_list = TodoList.find(params[:todo_list_id])
	end 

	def todo_item_params
		params[:todo_items].permit(:content)
	end
end
```

#### Dont have a form to actually create the todo. So go to views, create two partials under todoitems

1. '_form.html.erb':
```
<%= form_for([@todo_list, @todo_list.todo_items.build]) do |f| %>
	<%= f.text_field :content, placeholder: "New Todo" %>
	<%= f.submit %>
<% end %>
```

2. '_todo_item.html.erb':
```<p><%= todo_item.content %></p>```

#### Want to make our app show the form & todoitems under the todolist show page 
```
<div id="todo_items_wrapper">
	<!-- Will grab from _todo_item.html.erb -->
	<%= render @todo_list.todo_items %>
	<div id="form">
		<%= render "todo_items/form" %>
	</div>
</div>
```

#### Want the ability to delete a todo item 
1. In _todo_item.html.erb do
```
<%= link_to "Delete", todo_list_todo_item_path(@todo_list, todo_item.id), method: :delete, data: { confirm: "Are you sure?" } %>
```
2. Buttt that won't work yet, cause we dont have a method to destroy a todo item. Sooo we add it in the todo_item controller!
```
def destroy
	@todo_item = @todo_list.todo_items.find(params[:id])
	if @todo_item.destroy
		flash[:success] = "Todo List item was deleted"
	else
		flash[:error] = "Todo List item could not be deleted"
	end
	# After we hit delete, want it to route back to the /todo_lists page
	redirect_to @todo_list
end
```

#### Want to add completed_at:datetime to the todo_items model
```
rails g migration add_completed_at_to_todo_items completed_at:datetime
rake db:migrate
```

#### Want the ability to mark a todo item as complete. 

1. Add a route. Route looks like: 
```
resources :todo_lists do 
	resources :todo_items do
		member do
			patch :complete 
		end
	end
end
```

2. Show the option for every todo item
```
<%= link_to "Mark as Complete", complete_todo_list_todo_item_path(@todo_list, todo_item.id), method: :patch %>
```

3. Add complete method on todo_item controller!
```
def complete 
	@todo_item.update_attribute(:completed_at, Time.now)
	redirect_to @todo_list, notice: "Todo item completed"
end
```

#### Cleaning up _todo_item.html.erb
```
<div class="row clearfix">
<!-- Cant just use completed like that. Need to add in models -->
<% if todo_item.completed? %>
	<div class="complete">
		<%= link_to "Mark as Complete", complete_todo_list_todo_item_path(@todo_list, todo_item.id), method: :patch %>
	</div>
	<div class="todo_item">
		<p style="opacity: 0.4;"><strike><%= todo_item.content %></strike></p>
	</div>
	<div class="trash">
		<%= link_to "Delete", todo_list_todo_item_path(@todo_list, todo_item.id), method: :delete, data: { confirm: "Are you sure?" } %>
	</div>
	<% else %>
	<div class="complete">
		<%= link_to "Mark as Complete", complete_todo_list_todo_item_path(@todo_list, todo_item.id), method: :patch %>
	</div>
	<div class="todo_item">
		<p><%= todo_item.content %></p>
	</div>
	<div class="trash">
		<%= link_to "Delete", todo_list_todo_item_path(@todo_list, todo_item.id), method: :delete, data: { confirm: "Are you sure?" } %>
	</div>
<% end %>
```
#### To use the todo_item.completed? defined as above, add this in models under todo_item.rb
```
def completed?
	!completed_at.blank?
end
```

Now want to make it look muchhh nicer 

Ruby on Rails Lingoooo
- Gemfile: contents are to define which libraries (aka gem) we're gonna use 
- erb: default views of a rails app are erb files (Under app/views/layouts) 
- '_somefile.html.erb' in views: Means that its a partial. I.e. you might wanna reuse this chunk of code

#### Commands 
> bundle install: Download && Install the gems defined in Gemfile 

> rails server: Start the serverrr to run at localhost:3000

> rake routes: View current routes 

> rails generate controller pages: Generates a static pages controller 

> rails generate controller tasks: Generates a tasks controller

> rake db:migrate: Applying model migration

#### Rails code
> @tasks = Task.all > in app.html.erb: @ == variables? 

> resources :tasks > in routes.rb: this Task controller manages our model aka resource && we want it to handle CRUD actions on our model. i.e. this controller is resourceful

#### Creating table
> rails generate model Task title:string note:text completed:date: Generate a model Task, with following fields: title with type string, note with type text, completed with type date. 
Command output: 	
```
	invoke  active_record
	create    db/migrate/20170308110301_create_tasks.rb
	create    app/models/task.rb
	invoke    test_unit
	create      test/models/task_test.rb
	create      test/fixtures/tasks.yml
```

> 'db/migrate/xxxx_create_tasks.rb': is the migration file. Migration file changes db's schema upon execution. It has the commands to create the table for our tasks model. 
> 'app/models/task.rb': is the model. Its an empty subclass of the ActiveRecord::Base 

#### Adding row in table in rails console
> Task.create(title: 'First Task', note: 'This task was created inside the rails console')
```
	SQL (0.4ms)  INSERT INTO "tasks" ("title", "note", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "First Task"], ["note", "This task was created inside the rails console"], ["created_at", "2017-03-08 12:33:09.251782"], ["updated_at", "2017-03-08 12:33:09.251782"]]
```

#### Rails console: Adding & deleting the first task 
> task = Task.first 

> SQL Query: SELECT  "tasks".* FROM "tasks"  ORDER BY "tasks"."id" ASC LIMIT 1
```
=> #<Task id: 1, title: "First Task", note: "This task was created inside the rails console", completed: nil, created_at: "2017-03-08 12:33:09", updated_at: "2017-03-08 12:33:09">
```

> task.destroy
SQL Query: DELETE FROM "tasks" WHERE "tasks"."id" = ?  [["id", 1]]

> task.count
SQL Query: SELECT COUNT(*) FROM "tasks"
