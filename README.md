# Rails-Medium-Editor-Insert-Plugin
A simple and minimal editor for rails from [medium-editor-insert-plugin](https://github.com/orthes/medium-editor-insert-plugin)


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

## With ActiveStorage
You can make your own storage model, this is just a reference	
``url_for`` is an active_storage helper
		
	# Upload model

	class Upload < ApplicationRecord
	   has_one_attached :image
	end


	# UploadsController

	class UploadsController < ApplicationController
	  def uploader
	    @image = Upload.new(image: params[:files].first)
	    url_response = {
	      files: [
	        {
		  url: url_for(@image.image),
		  thumbnail_url: url_for(@image.image),
		  type: "image/jpeg",
		  size: 0
	        }
	      ]
	      }
	    render json: url_response
	  end
	end

## ActiveStorage with Cloudinary
Use this gem
[ActiveStorage cloudinary service gem](https://github.com/0sc/activestorage-cloudinary-service)

	gem 'cloudinary', require: false
	gem 'activestorage-cloudinary-service'


and in your storage_yml. Add 'options:' if you want to serve your assests in https.
	
	cloudinary:
 	  service: Cloudinary
  	  cloud_name: <%= ENV['CLOUDINARY_CLOUD_NAME'] %>
  	  api_key:    <%= ENV['CLOUDINARY_API_KEY'] %>
  	  api_secret: <%= ENV['CLOUDINARY_API_SECRET'] %>
  	  options:
  	    secure: true
  	    cdn_subdomain: true



## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/rails_medium_editor_insert_plugin. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).


