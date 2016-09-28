# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
Connects two tables through one or more relationships.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
class Profiles
  attributes :given_name, :surname, :email
end

class Movies
  attributes :title, :release_date, :length
end

class Favorites
  :given_name, :surname, :email, :title, :release_date, :length, :profiles, :movies
end
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites, dependent: :destroy
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :profile, inverse_of: :favorites
  belongs_to :movie, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
The serializer one sets, depends on the use-case. You can set it to whatever
your app requires so it shows the user the params, columns, tables, you want
the user to see..
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :id, :movies, through: :favorites, :movies
  has_many :favorites
end

```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
bundle exec rails g scaffold Favorites profile:references movie:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
It deletes a column of a table, even if it has an association with it so its
reference to the other gets deleted as well.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
A movie streaming application
One to one :  an user has a profile and a profile has a user.
Many to many : there are many movies to many profiles.
```
