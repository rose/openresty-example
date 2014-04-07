To try for yourself:
1. [Sign up](https://signup.heroku.com/signup/dc) for a Heroku account
2. Get the [Heroku Toolbelt](https://toolbelt.heroku.com/) (easy to install)
3. Clone this repository:
    $ git clone https://github.com/davidad/openresty-heroku-example
    $ cd openresty-heroku-example
4. Think up a crazy app name that won't already be taken. I'll use `crazy-999` here.
    $ heroku create crazy-999 --buildpack=https://github.com/davidad/heroku-buildpack-openresty.git
