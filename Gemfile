source "https://rubygems.org"

# This is the default theme for new Jekyll sites. 
gem "jekyll-remote-theme"

group :jekyll_plugins do
  gem "github-pages","~> 207"
  gem "jekyll-feed", "~> 0.12"
  gem 'jekyll-admin', "~> 0.10.1"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
