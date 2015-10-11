# Data layer. Migrations

## Example migration

```
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
 
      t.timestamps
    end
  end
end

```

#### In other words

```
class CreateUsers < ActiveRecord::Migration
  def up
    create_table :users do |t|
      t.string :name
 
      t.timestamps
    end
  end
  
  def down
    drop_table :users
  end
end

```

## Naming conventions

```
rails g migration CreateUsers name:string
rails g migration AddAgeToUsers age:integer
rails g migration RemoveAgeFromUsers age:integer

rails generate model Pet name:string age:integer

```

## Rake tasks

```
rake db:create
rake db:drop

rake db:migrate
rake db:rollback

rake db:seed
rake db:migrate:status
rake db:version
rake db:reset # Warning! It does not do what it is intended!

rake db:migrate RAILS_ENV=test

```

## Trace migration

* suppress_messages	
  * Takes a block as an argument and suppresses any output generated by the block.
* say	
  * Takes a message argument and outputs it as is. A second boolean argument can be passed to specify whether to indent or not.
* say_with_time	
  * Outputs text along with how long it took to run its block. If the block returns an integer it assumes it is the number of rows affected.
  
```
class CreateCats < ActiveRecord::Migration
  def change
    suppress_messages do
      create_table :cats do |t|
        t.string :name
        t.integer :age
        t.timestamps
      end
    end
 
    say "Cats are here"
 
    suppress_messages {add_index :cats, :name}
    say "index, index, index!", true # Say with indentation
 
    say_with_time '333 ghost cats in 3 seconds!' do
      sleep 3
      333
    end
  end
end

```

## Do not touch me!

You should not change the migrations that were pushed!



# Object-relational mapping (ORM)

* maps objects to database and hides the complexity
* many implementations
* many approaches

# DataMapper pattern (Perpetuity)

Object <-> ObjectMapper <-> DataBase

```
class Article
  attr_accessor :title, :body
end

Perpetuity.generate_mapper_for Article do
  attribute :title
  attribute :body
end

article = Article.new
article.title = 'New Article'
article.body = 'This is an article.'

Perpetuity[Article].insert article
```

# Active record or ActiveRecord?

## Active record pattern

* table wrapped into a class
* table row wrapped intro a class instance
* all further actions are related to the database

## ActiveRecord

`ActiveRecord` is an implementation of Active record pattern

#### Example
```
class User < ActiveRecord::Base
end
```

# Seeds

* simple example
* split into files
* handle ordering by proper naming