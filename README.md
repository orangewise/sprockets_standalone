# Sprockets Standalone

Using Sprockets and AssetSync standalone, so it can be in combination with a PHP website :) 

Based on http://www.simonecarletti.com/blog/2011/09/using-sprockets-without-a-railsrack-project/




# Usage

	bundle exec rake sprockets:compile environment=dev
	
	or
	
	bundle exec rake sync:assets environment=dev
	




# Directory structure

	.
	├── sprockets_standalone
	|		└── config
	|       └── asset_sync.yml
	|		├── Gemfile
	|		├── Gemfile.lock
	|		├── Rakefile
	└── assets
			├── dev
			|   └── build
			│        ├── javascripts
			│        │   └── all.js
			│        └── stylesheets
			│            └── all.css
			├── test
			|   └── build
			|        ├── ...
			├── production
			|   └── build
			|        ├── ...
			└── src
			    ├── javascripts
			    │   └── all.js
			    |   └── jquery
			    |       └── jquery-1.10.2.min.js
			    └── stylesheets
			        └── all.css






# Example all.js

	//= require jquery/jquery-1.10.2.min
	
	
	
	
# Example all.css

	/*
	 *= require bootstrap/index
	 */
