### Controllers

We've got a lot going on in our controllers -- querying, authorizing,
decorating, business logic, etc. In order to keep all of that activity
organized, let's do things in the following order:

1. Finders (if necessary)
2. Authorization
3. Business Logic
4. Decoration
5. Rendering

```ruby
def action
  # 1. Finders (if necessary)
  @logbook = Logbook.find(params[:id])

  # 2. Authorization
  authorize @logbook

  # 3. Business Logic
  @logbook.foo = "bar"
  @logbook.save!

  # 4. Decoration
  @logbook = @logbook.decorate

  # 5. Rendering
  render 'baz'
end
```

### Scopes

AR query methods can be written either as class methods or using the `scope` macro.  E.g.

```ruby
def self.some_scope(param)
  if param.present?
    where(param: param)
  else
    self.all
  end
end
```

```ruby
scope :some_scope, ->(param) { where(param: param) if param }
```

Either form is acceptable, but we'll prefer using scopes because they handle nil checks more gracefully (as demonstrated above), and because that's the direction the community's headed.
