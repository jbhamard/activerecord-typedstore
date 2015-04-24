# Typed Store Accessors
## Dependencies
Activerecord 4.2.0

pg (postgres gem)

## Objectives

Typed store accessors should allow defining typed attributes in JSON object saved
into postgres JSON type columns.


## Declaration in Rails models

```ruby
class Foo < ActiveRecord::Base
  #declare typed accessors on the JSON object of the bar attribute
  #bar value is saved into a JSON postgres column

  typed_store_accessors :bar do |t|
    #typed JSON attributes
    #use regular model migrations declaration
    #all rails attributes types are supported
    t.string :str, default: 'test', null: false
    t.integer :int
    t.boolean :bool, default: false
  end
end
```

## Exposed model accessors

Accessors are available on the JSON model attribute.


```ruby
@foo.bar.str= 'string'
@foo.bar.int= 10
@foo.bar.bool= true
```

The entire attribute value can also be set with a Hash.
Extra keys are allowed. This replaces the old Hash, no merging.

```ruby
@foo.bar= {str: 'string', int:10, bool: true, other: 'other_value'}
```


##Type casting behavior

The type casting behavior should be the same as for regular Rails model attributes.


##Strategy

+ Use activerecord-typedstore like DSL for accessors definition
+ Set accessors methods :
  + use Activerecord type cast
  + update current attribute Hash
  + let Activerecord set the Hash
+ Catch model attribute setter calls :
  + cast JSON values according to DSL
  + let Activerecord set the Hash
