# Ruby on Rails

* MVC Review
* Quickly build a simple Rails app

### MVC

* Model - manages the data; abstraction
* View - templating engine (ERB) / helper functions
* Controller - brains of the operation

* Router directs all the traffic

gem 'faker' gets fake data for your database!

### Build Simple Rails App

Terminal:
rails new hey-arnold // installs project with all necessary gems and files/folders
rails s // starts up server

* Gemfile
  * where all our dependencies are kept
  * includes the source of the gems
  * includes the version of ruby we're using
  * would include 'faker' in the group :development section of gemfile

* bundle install is the equivalent of npm install

* instead of creating a file, we're going to use 'rails generate' in the terminal
  * rails generate model location // creates locations table (look below)
  * rails generate model character // creates characters table (look below)

  ```ruby
  class CreateLocations < ActiveRecord::Migration[6.1]
    def change
      create_table :locations do |t|
        t.string :name

        t.timestamps
      end
    end
  end
  ```
  * creates a table called locations that stores names and timestamps
  * 't' is refering to the table

  ```ruby
  class CreateCharacters < ActiveRecord::Migration[6.1]
    def change
      create_table :characters do |t|
        t.string :name
        t.string :quote
        t.references :location, foreign_key: true, index: true # connects this table to the locations table - always index foreign keys
        t.timestamps
      end
    end
  end
  ```

  Terminal:
  rails db:migrate // goes through and runs your migrations

  location.rb:
  ```rb
  class Location < ApplicationRecord
    has_many :characters  # adds this method to every location
  end
  ```
  character.rb:
  ```rb
  class Character < ApplicationRecord
    belongs_to :location
  end
  ```

  * seeds go into db/seeds.rb
    * just ONE file for all of your seeds
    * location.create creates the row on the table and saves it
    * Location.create(
      name: Faker::TvShows::HeyArnold.location()
    )
      * you can create a loop here to make a whole bunch of location rows
      * 10.times do
          Location.create(
            name: Faker::TvShows::HeyArnold.location()
          )
        end
      * would create 10 location rows
    
    * 50.times do
      Character.create(
        name: Faker::TvShows::HeyArnold.character,
        quote: Faker::TvShows::HeyArnold.quote,
        location: locations.sample // would give back a random location
      )

Terminal:
rails db:seed // runs seed files

In routes.rb:
```rb
Rails.application.routes.draw do

  resources: locations

end
```

* creates 8 locations routes for us! (ie. get locations, get locations/id, post locations, etc)