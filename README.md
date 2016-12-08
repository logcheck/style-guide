# LogCheck Style Guide


## About this guide


### Omit needless words

The goal of this guide is to help you write source code that is
readable, understandable, and maintainable by your peers. If it were
possible to achieve this goal through a strict adherence to
well-defined rules, there would be no need for this guide.

A good style guide provides _guidance_, but acknowledges that you and
your peers are the ultimate arbiter of what is readable and what is
not.


## General Guidelines


### Write readable code.

This is the most important rule.

A readable program can always be improved. If it has a syntax error,
it can be corrected. If it has bugs, they can be fixed. If it is slow,
it can be optimized. If it is insufficient, it can be enhanced.

The less readable a program is, the harder it is to change it. An
unreadable program in any language might as well be written in machine
code.


### Avoid superfluous whitespace.

Trailing whitespace characters should be deleted.  Blank lines should
be empty. Files should end with a single `\n`.

Superfluous invisible characters make for noisy diffs.


## Guidelines for Ruby


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