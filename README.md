# Live Deployment

Visit: [https://ouassil.allak.fr](https://ouassil.allak.fr)

# Local Deployment

*Jekyll* is a ruby thing. First install install *ruby* and *gem*, on *Archlinux*: 

```
sudo pacman -S ruby rubygems
```

Then install install gems *jekyll* and *bundler*:

```
gem install jekyll bundler
```

Then change directory to cloning path of this repository and: 

```
bundle install
bundle add webrick
bundle exec jekyll serve --livereload
```

Visit: [http://127.0.0.1:4000/](http://127.0.0.1:4000/)
