## Info
Jekyll theme for my website with no JavaScript, no hidden requests, no/less bloat.

## Installation
#### Install these programs first
```sh
# On Debian (Ubuntu, Mint, etc)
sudo apt install zlib1g-dev ruby-full build-essential

# On Arch (Arco, Endeavor, etc)
sudo pacman -Syu zlib1g-dev ruby-full build-essential
```
#### Now you need to install jekyll and bunder
```sh 
# Install these dependencies (it can take a while to build the native dependencies)
sudo gem install jekyll bundler
```
#### After a while when everything is done, run the command provided below
```sh 
bundle exec jekyll serve
# Now open your web browser and type localhost:4000 or 127.0.0.1:4000 to see the deployed website
```
