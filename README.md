# Rails-Medium-Editor-Insert-Plugin

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rails_medium_editor_insert_plugin'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rails_medium_editor_insert_plugin

## Usage

This gem need a font-awesome gem

    $ gem "font-awesome-rails"

after installing gem add this into your:

/assets/javascripts/application.js

    //= require medium-editor-js

/assets/stylesheets/applications.css

    *= require medium-editor-style
    *= require font-awesome


## Using this gem in you rails app


## Handle image uploaing
You can use any of this gem  
[carrierwave link](https://www.google.com)

Create a method inside of you controller like this.


  def upload
    image = current_user.posts.new(image: params[:files].first)
    image.save
    url_response = {
      files: [
        {
          url: @image.image.url,
          thumbnail_url: @image.image.url,
          name: @image.image_identifier,
          type: "image/jpeg",
          size: 0
        }
      ]
    }
    render :json => url_response
  end

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/rails_medium_editor_insert_plugin. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).


