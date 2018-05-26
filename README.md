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

Inside of your rails form

    <%= form_with model: @post, class: 'editor-form', local: true  do |post|%>
      # forn_with is for rails 5+
      <div class="field">
        <div class="control">
          <%= post.text_area :body, class: 'editable'%> 
        </div>
      </div>

      <%= post.submit 'Publish', class: 'button is-primary is-rounded'%>

    <% end %>

and initialize your editor
  
    $(document).on('turbolinks:load', function(){

      var editor = new MediumEditor('.editable',{
         placeholder: {
              text: "Write a story"
            }
      });
      

      $(function () {
        $('.editable').mediumInsert({
            editor: editor,
            enabled: true,

            addons: {
              images: {
                fileUploadOptions: {
                  url: '/images/upload', // your uploading image routes: post "images/upload" => "posts#upload"
                  type: 'post',
                  acceptFileTypes: /(.|\/)(gif|jpe?g|png)$/i
                },
                  fileDeleteOptions: {
                  url: '/image/delete',
                  type: 'delete'
                }
              }
            }
        });
      });
    })

      
## Add image upload in your rails app controller

You can use any of this gem for file upload

[carrierwave link](https://github.com/carrierwaveuploader/carrierwave)

[paperclip link](https://github.com/thoughtbot/paperclip)


Create a method inside of you controller like this.



    def upload
      # image = current_user.posts.new(image: params[:files].first) -> // if you are using devise 
      image = Posts.new(image: params[:files].first)
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

Your routes should something like this
    
    post "images/upload" => "posts#upload"


This code block comes from this man [ mwlang ](https://github.com/mwlang/medium-editor-insert-plugin-rails) big thanks to him

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/rails_medium_editor_insert_plugin. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).


