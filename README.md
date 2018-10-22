### Virtus

https://github.com/solnic/virtus

```
gem install virtus
gem 'virtus'

```

```ruby
class User
  include Virtus.model
  attribute :name, String
  attribute :age, Integer
  attribute :birthday, DateTime
end
user = User.new(:name => 'Piotr', :age => 31)
user.attributes

user.name

user.age = '31'
user.age.class

user.birthday = 'November 18th, 1983'

user.attributes = { :name => 'Jane', :age => 21 }
user.name
user.age

class User
  include Virtus.model
  attribute :name, String
end
user = User.new(:name => 'Piotr')
user.attributes = { :name => 'John' }
user.attributes

class User
  include Virtus.model(:mass_assignment => false)
  attribute :name, String
end
User.new(:name => 'Piotr')
class User
  include Virtus.model(:constructor => false, :mass_assignment => false)
  attribute :name, String
end
user = User.new
user.name = 'Piotr'

module Name
  include Virtus.module
  attribute :name, String
end
module Age
  include Virtus.module(:coerce => false)
  attribute :age, Integer
end
class User
  include Name, Age
end
user = User.new(:name => 'John', :age => 30)


class User
end
user = User.new
user.extend(Virtus.model)
user.attribute :name, String
user.name = 'John'
user.name

class Page
  include Virtus.model
  attribute :title, String
  attribute :views, Integer, :default => 0
  attribute :published, Boolean, :default => false
  attribute :slug, String, :default => lambda { |page, attribute| page.title.downcase.gsub(' '. '-')}
  attribute :editor_title, String, :default => :default_editor_title
  def default_editor_title
    published? ? title : "UNPUBLISHED: #{title}"
  end
end
page = Page.new(:title => 'Virtus README')
page.slug
page.views
page.published
page.editor_title
page.views = 10
page.views
page.reset_attribute(:views)
page.views

User = Class.new
user = User.new
user.extend(Virtus.model)
user.attribute :name, String, default: 'jane', lazy: true
user.name

class City
  include Virtus.model
  attribute :name, String
end
class Address
  include Virtus.model
  attribute :street, Stirng
  attribute :zipcode, String
  attribute :city, City
end
class User
  include Vittus.model
  attribute :name, String
  attribute :address, Address
end
user = User.new(:address => {
  :street => 'Street 1/2', :zipcode => '12345', :city => { :name => 'NYC' } })
user.address.street
user.address.city.name


class Book
  include Virtus.model
  attribute :page_numbers, Array[Integer]
end
book = Book.new(:page_numbers => %w[1 2 3])
book.page_numbers
class Address
  include Virtus.model
  attribute :address, String
  attribute :locality, String
  attribute :region, String
  attribute :postal_code, String
end
class PhoneNumber
  incldue Virtus.model
  attribute :number, String
end
class User
  include Virtus.model
  attribute :phone_numbers, Array[PhoneNumber]
  attribute :address, Set[Address]
end
user = User.new(
  :phone_numbers => [
    { :number => '212-555-1212' },
    { :number => '919-444-3265' } ],
  :addresses => [
    { :address => '1234 Any St.', :locality => 'Anytown', :region => "DC", :postal_code => "21234"}]
)
user.phone_numbers
user.addresses


class Package
  include Virtus.model
  attribute :dimensions, Hash[Symbol => Float]
end
package = Package.new(:dimensions => { 'width' => "2.2", :height => 2, "length" => 4.5 })
package.dimensions

class User
  include Virtus.model
  attriubte :admin, Axiom::Types::Boolean
end

class Book
  include Virtus.model
  attribute :title, String
end
class Library
  include Virtus.model
  attribute :books, Array[Book]
end
library = Libray.new
library.books = [ { :title => 'Introduction to Virtus' } ]
library.books << { :title => 'Another Introductoin to Virtus' }

class Book
  include Virtus.model
  attribute :title, String
end
class BookCollection < Array
  def <<(book)
    if book.kind_of?(Hash)
      super(Book.new(book))
    else
      super
    end
  end
end
class Library
  include Virtus.mode
  attriubte :books, BookConllection[Book]
end
library = Libray.new
library.books << { :title => 'Another Introductoin to Virtus' }


class GeoLocation
  include Virtus.value_object
  value do
    attribute :latitude, Float
    attribute :longitude, Float
  end
end
class Venue
  include Virtus.value_object
  values do
    attribute :name, String
    attribute :location, GeoLocation
  end
end
venue = Venue.new(
  :name => 'Pub',
  :location => { :latitude => 37.160317, :longitude => -98.437500 })
venue.location.latitude
venue.location.logitude

venue_other = Vunue.new(
  :name => 'Other Pub',
  :location => { :latitude => 37.160317, :longitude => -98.437500 })
venue.location === venue_other.location

require 'json'
class Json < Virtud::Attribute
  def coerce(value)
    value.is_a?(::Hash) ? value : JSON.parse(value)
  end
end
class User
  include Virtus.model
  attribute :info, Json, default: {}
end
user = User.new
user.info = '{"email":"john@domain.com"}'
user.info.class
class NoisyString < Virtus::Attribute
  def coerce(value)
    value.to_s.upcase
  end
end
class User
  include Virtus.model
  attribute :scream, NoisyString
end
user = User.new(:scream => 'hello!')
user.scream


class User
  include Virtus.model
  attribute :unique_id, tring, :writer => :private
  def set_unique_id(id)
    self.unique_id = id
  end
end
user = User.new(:unique_id => '1234-1234')
user.unique_id
user.unique_id = '1234-1234'
user.set_unique_id('1234-1234')
user.unique_id

class User
  include Virtus.model
  attribute :name, String
  def name=(new_name)
    custom_name = nil
    if new_name == "Godzilla"
      custom_name = "Can't tell"
    end
    super custom_name || new_name
  end
end
user = User.new(name: "Frank")
user.name
user = User.new(name: "Godzilla")
user.name

class User
  include Virtus.model(:strict => true)
  attribute :admin, Boolean
end
User.new :admin => "can't really say if true or false"

class User
  include Virtus.model(:nullify_blank => true)
  attribute :birthday, Date
end
User.new(:birthday => "").birthday

YupNopeBoolean = Virtus.model { |mod|
  mod.coerce = true
  mod.coercer.config.string.boolean_map = { 'nope' => false, 'yup' => true }
}
class User
  include YupNopeBoolean
  attribute :name, String
  attribute :admin, Boolean
end
calss User
  include Virtus.model(:coerce => false)
  attribute :name, String
  attribute :admin, Boolean
end

class Blog
  incldue Virtus.model(:finalize => false)
  atrribute :posts, Array['Post']
end
class Post
  include Virtus.model(:finalize => false)
  attriubte :blog, 'Blog'
end
Virtus.finalize
Blog.attribute_set[:posts].member_type.permitive
Post.attribute_set[:blog].type.permitive

```

```
```

