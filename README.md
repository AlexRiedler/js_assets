# JavaScript Asset Compiler
Precompile assets built with JavaScript compilers, for example mustache templates compiled by `handlebars.js`.

Supports:
- Rails 4
- Rails 3.2

# Install

add
```ruby
gem 'js_assets'
```
to your Gemfile

## Rails

Nothing special to do

## Sprockets (non-rails)
Add the asset pipeline to your sprockets environment

```ruby
env = Sprockets::Environment.new # Use an existing one if it exists

require 'js_assets'
env.append_path JsAssets.path
```

## Tilt/ActionView

TODO

# Configuration

## Rails

### Adding a compiler

NOTE: this is experimental syntax (not implemented)
e.g. Handlebars
```ruby
  class HandlebarsJsAssetCompiler extends JsAssets::Compiler

    # The list of javascript files
    # loaded into the JS environment before compilation
    # useful for patching the environment (e.g. registering helpers)
    def compiler_js_environment
      ["app/assets/javascripts/lib/handlebars.js"]
    end

    # Prefix all compilations with the return string
    def prefix_template
      "(function() {"
    end

    # Returns javascript string on how to compile source
    #
    # source_string is a javascript escaped and quoted string.
    def compile_source(source_string)
      "Handlebars.compile(#{source_string})"
    end

    # Returns javascript string on how to register a partial
    #
    # template_name, e.g. "books/index"
    # precompiled_template, e.g. "Handlebars.compile('<escaped source of file')"
    def register_partial(template_name, precompiled_template)
      "Handlebars.registerPartial(#{template_name}, #{precompiled_template})"
    end

    # Postfix all compilations with the returned string
    def postfix_template
      "}).call(this)"
    end

  end

  # Registers .hbs extension with the tilt + actionview pipelines (if available)
  JsAssets::Config.register_handler('.hbs', HandlebarsJsAssetsCompiler)
```

### Template Namespace

You can adjust the namespace in which JS templates are compiled into.
Commonly JST is used.

```ruby
JsAssets::Config.template_namespace = 'JST'
```

if you had a file e.g. `app/assets/javascripts/templates/books/index.hbs`
It's precompiled template function would then be available at `JST['books/index']` in your Javascript.

### Template Directory

The recommended folder is `app/assets/javascripts/templates`
In order to include templates

e.g. app/assets/javascripts/app.js
```javascript
//= require_tree ./templates
```
Be sure to include AFTER your template library.

# Contribute

- Discuss (if necessary)
- Fork + Branch
- Pull Request
- Discuss

