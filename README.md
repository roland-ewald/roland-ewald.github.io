## Personal Website

### How to run

Run a local build with `bundle exec jekyll serve`

### How to publish

Merge changes to `master` branch and push to Github.

### How to set up

```bash
sudo apt install ruby-full
# if this does not work because rubygems cannot be reached, adjust IPv6
# setting for this IP in /etc/gai.conf (see https://stackoverflow.com/a/50349235/109942)
gem install bundler
bundle config set --local path .bundle/gems
bundle install
# For updates
bundle update
bundle lock
```
