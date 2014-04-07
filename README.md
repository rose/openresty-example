Getting started with the glorious `nginx`+[LuaJIT](http://luajit.org) Web development platform [OpenResty](http://openresty.org) is easy with my [OpenResty Heroku buildpack](https://github.com/davidad/heroku-buildpack-openresty)! All you need is a file called `conf/openresty.conf`, as demonstrated in this minimal "hello world"-style app. You don't even _need_ to install OpenResty locally.

To try this example app for yourself:
1. [Sign up](https://signup.heroku.com/signup/dc) for a Heroku account
2. Get the [Heroku Toolbelt](https://toolbelt.heroku.com/) (easy to install)
3. Clone this repository:
    $ git clone https://github.com/davidad/openresty-heroku-example
    $ cd openresty-heroku-example
4. Think up a crazy app name that won't already be taken. I'll use `example-999` here, but that'll probably be taken, so pick some different numbers or something.
    $ heroku create example-999 --buildpack=https://github.com/davidad/heroku-buildpack-openresty.git
5. Deploy the app to Heroku.
    $ git push heroku master
6. Observe that `http://example-999.herokuapp.com/` looks like [this](http://openresty-heroku-example.herokuapp.com/).

