# Spree Essential CMS [![Build Status](https://secure.travis-ci.org/citrus/spree_essential_cms.png)](http://travis-ci.org/citrus/spree_essential_cms)


Installation
------------

If you don't already have an existing Spree site, [click here](https://gist.github.com/946719) then come back later... You can also read the Spree docs [here](http://spreecommerce.com/documentation/getting_started.html)...

Otherwise, follow these steps to get up and running with SpreeEssentialCms:

Add spree_essential_cms to your Gemfile:

    gem 'spree_essential_cms', '>= 0.2.0'

Now, bundle up with:

    bundle install

Then run the generators to create the migration files:

    rails g spree_essentials:install
    rails g spree_essentials:cms

Now migrate your database:

    rake db:migrate
    
Boot your server and checkout the admin!

    rails s
    


Usage
-----

[todo] add basic usage stuff here


### Here's some tips for making content really customized...


#### Contexts

Contexts allow you to place different forms of content in various places of a page. A slideshow or a sidebar might be good examples...

You can set a content's context (say that ten times fast!) under the 'Optional Fields' tab in the edit content view.

In your view you'll be able to grab those bits of content like so:

    
    .slideshow
      - if @slides = @page.for_context('slideshow')
        = render 'shared/content', :content => @slides
    
    - if @sidebar = @page.for_context('sidebar').first
      - content_for :sidebar do
        = render 'shared/your_custom_sidebar', :content => @sidebar  




#### Custom image sizes

Use a content decorator when you want different contexts to have their own image sizes:


    Content.class_eval do 
    
      # override default image sizes
      def default_attachment_sizes
        { :mini => '48x48>', :medium => '427x287>' }
      end
      
      # or set a custom size for each context
      def attachment_sizes
        case context
          when 'slideshow'
            sizes = default_attachment_sizes.merge(:slide => '955x476#')
          when 'main'
            sizes = default_attachment_sizes.merge(:custom => '580x289#')
          when 'small-top'
            sizes = default_attachment_sizes.merge(:custom => '364x177#')
          when 'small-bottom'
            sizes = default_attachment_sizes.merge(:custom => '364x109#') 
          else
            sizes = default_attachment_sizes
        end
        sizes
      end
    
    end





    
Demo
----

You can easily use the test/dummy app as a demo of spree_essential_cms. Just `cd` to where you develop and run:
    
    git clone git://github.com/citrus/spree_essential_cms.git
    cd spree_essential_cms
    mv lib/dummy_hooks/after_migrate.rb.sample lib/dummy_hooks/after_migrate.rb
    bundle install
    bundle exec dummier
    cd test/dummy
    rails s
    

Change Log
----------

**0.2.0 - 2011/12/15**

* Add 0.70.x compatibility
* Removed spork dev dependency


**0.1.1 - 2011/6/2**

* Added the image_size option `shared/_content.html.erb`
* Improved documentation


**0.1.0 - 2011/6/1**

* Random cleanup
* Released 0.1.0 to rubygems


**2011/4/26**

* Extracted from the Spree Essentials core.


To Do
-----

* more tests... many many more.
* optimizations
* A 'create translation' button that clones the current page's contents into another language
* add widgets that you can drop into any page
* page and menu caching/sweeping
* nested set for pages
* 0.30 and 0.40 compatibility
* create wiki pages


Contributors
------------

So far it's just me; Spencer Steffen. 

If you'd like to help out feel free to fork and send me pull requests!


License
-------

Copyright (c) 2011 Spencer Steffen & Citrus, released under the New BSD License All rights reserved.
