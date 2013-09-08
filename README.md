# Sprockets Standalone

Based on http://www.simonecarletti.com/blog/2011/09/using-sprockets-without-a-railsrack-project/

# Usage

	bundle exec rake compile

# Directory structure

	.
	├── sprockets_standalone
	|		└── src
	|		├── Gemfile
	|		├── Gemfile.lock
	|		├── Rakefile
	└── assets
			├── build
			│   ├── javascripts
			│   │   └── all.js
			│   └── stylesheets
			│       └── all.css
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
