# frozen_string_literal: true

source "https://rubygems.org"

# Choo choo 🚝 (only include the Rails gems we need)
gem "actionpack", ">= 6.1.4.2"
gem "actionview"
gem "activesupport"
gem "railties", ">= 6.1.4.2"
gem "sprockets-rails", ">= 3.2.2"

# Use Puma as the app server
gem "puma"

# Use SCSS for stylesheets
gem "sass-rails", ">= 6.0.0"

# Use Uglifier as compressor for JavaScript assets
gem "uglifier"

# Helps with running the server locally
gem "foreman"

# For converting strings to URL friendly versions
gem "stringex"

# Rendering markdown
gem "redcarpet"

# Parsing markdown
gem "commonmarker"

# Syntax highlighting code
gem "rouge", "3.3.0"

# For escaping code snippets in markdown
gem "escape_utils"

# One rails log line per request, instead of enraging quantity
gem "lograge", ">= 0.11.2"

# Error reporting
gem "bugsnag"

group :development, :test do
  # Call "byebug" anywhere in the code to stop execution and get a debugger console
  gem "byebug", platforms: [:mri, :mingw, :x64_mingw]
end

group :development do
  # Access an interactive console on exception pages or by calling `console` anywhere in the code.
  gem "web-console", ">= 4.1.0"
  gem "listen"
  gem "pry"
end

group :test do
  # Who doesn't love tests!?
  gem "rspec-rails", ">= 5.0.1"

  # We want junit output so we can annotate the build
  gem "rspec_junit_formatter"

  # Browser testing stuff
  gem "capybara"
end
