source "https://rubygems.org"

gem "jekyll", "~> 4.0"
gem "minimal-mistakes-jekyll"

# Plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-gist"
  gem "jekyll-octicons"
  gem "jekyll-github-metadata"
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-include-cache"
  gem "jekyll-seo-tag"
  gem "jekyll-remote-theme"
end

# Windows and JRuby
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :install_if => Gem.win_platform?
gem "faraday", "< 1.0"