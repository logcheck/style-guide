## Controllers

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

## Scopes

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

## Views

There are a number of options to simplify and organize code in the view layer. We currently use Partials, Helpers, Presenters, and Decorators. Unfortunately it's not always clear which technology to use in a given situation. This section is meant to help clarify which technology to choose while writing code and reviewing Pull Requests.

This current iteration is copy/pasted from [When I use Helpers, Partials, Presenters and Decorators](https://coderwall.com/p/jx9tca/when-i-use-helpers-partials-presenters-and-decorators) with modifications for grammar, formatting, and how we work at LogCheck.

### Helpers
Helpers are generic methods which can be used for different kinds of objects. These methods can create html with css style or just standard text.

Examples include:
- link_to_update
- big_image
- styled_form

### Partials
Partials are used to split a big view into smaller logicical parts and for larger html code.

Examples include:
- side_menu
- comment_list
- header

### Presenters
Presenters are for more complicated queries with two or more models.

Examples include:
- @page_presenter.page_in_category(ruby_category)
- @user_presenter.user_following(an_article)

### Decorators

Decorators should act with only one model and shouldn't take parameters (if possible). We use the [Draper](https://github.com/drapergem/draper) gem to implement decorator support.

Examples include:
- user.full_name
- page.big_title
- category.permalink
