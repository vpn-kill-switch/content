# How to contribute

You can contribute in multiple ways, submiting code, creating a post, update
existing posts, etc.

## Creating & updating posts

You can clone the full site by doing this:

First install HUGO: https://gohugo.io/overview/installing/

    $ brew update && brew install hugo

Clone the template repository:

```sh
$ git clone git@github.com:vpn-kill-switch/template.git $HOME/vpn-kill-switch.com
```

Clone the content for the site as a submodule:

```sh
$ cd $HOME/vpn-kill-switch.com
$ git submodule add -b master git@github.com:vpn-kill-switch/content.git content
```

Symlink the static directory so images can be displayed:

```sh
$ cd $HOME/vpn-kill-switch.com
$ ln -s content/static .
```

Start the server:

```sh
$ cd $HOME/vpn-kill-switch.com
$ hugo -w server
```

Create a post:

    $ hugo new post/example.md

Preview:

```sh
$ open http://localhost:1313/
```


## pull request

After adding your post or making any changes to the content repository please
create a pull request, more info about how to do this can be found here:
https://help.github.com/articles/creating-a-pull-request/ Please create a
pull-request for all your changes in order to review them and

# Contribute with code

The best for now is to create a repository in
your own account on Github and notify via an
[issue](https://github.com/vpn-kill-switch/vpn-kill-switch.github.io/issues)
when it is ready.
