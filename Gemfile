source "https://rubygems.org"

# Local development build. GitHub Pages builds the published site with its own
# pinned environment; these plugins are all on the GitHub Pages supported list,
# so the site renders the same way when deployed.
gem "jekyll", "~> 4.3"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.17"
  gem "jekyll-seo-tag", "~> 2.9"
  gem "jekyll-sitemap", "~> 1.4"
end

# Webrick is no longer bundled with Ruby 3+, needed for `jekyll serve`.
gem "webrick", "~> 1.9"

# Tzinfo data for platforms without a system zoneinfo database.
gem "tzinfo-data", platforms: [:windows, :jruby]
