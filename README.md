# Astralway website

Code powering the Astralway website ([http://astralway.github.io](http://astralway.github.io)).

# Contributions

Contributions to the website can be made by submitting pull requests to this repo.

If you want to view your changes in your browser before submitting a pull request, 
you will need install all of the gems in the [Gemfile] to serve the website in your
browser using [Jekyll]. This can be done by following these instructions:

1. After you have Ruby and RubyGems installed on your machine, install [Bundler]:

        gem install bundler

2. Use [Bundler] to install all gems in the [Gemfile] of this repo.

        cd incubator-fluo-website/
        bundle install

3. Run the following command to have Jekyll serve the website locally:

        bundle exec jekyll serve --watch

4. Open your web browser to [http://localhost:4000](http://localhost:4000).

# Powered by

This website runs on [Jekyll] using the [Hyde] template.

[Jekyll]: http://jekyllrb.com/
[Bundler]: http://bundler.io/
[Gemfile]: Gemfile
[Hyde]: https://github.com/poole/hyde
