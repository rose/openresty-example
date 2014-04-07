Getting started with the glorious [nginx](http://nginx.org)+[LuaJIT](http://luajit.org) Web development platform [OpenResty](http://openresty.org) is easy with my OpenResty Heroku [buildpack](https://github.com/davidad/heroku-buildpack-openresty)! All you need is a file called `conf/openresty.conf`, as demonstrated in this minimal "hello world"-style app. You don't even _need_ to install OpenResty locally.

To try this example app for yourself:
  1. [Sign up](https://signup.heroku.com/signup/dc) for a Heroku account
  2. Get the [Heroku Toolbelt](https://toolbelt.heroku.com/) (easy to install)
  3. Clone this repository and `cd` into it:<br>
     `$ git clone https://github.com/davidad/openresty-heroku-example; cd openresty-heroku-example`
  4. Think up a crazy app name that won't already be taken. I'll use `example-999` here, but that'll probably be taken, so pick some different numbers or something.<br>
     `$ heroku create example-999 --buildpack=https://github.com/davidad/heroku-buildpack-openresty.git`
  5. Deploy the app to Heroku.<br>
     `$ git push heroku master`
  6. Observe that `http://example-999.herokuapp.com/` looks like [this](http://openresty-heroku-example.herokuapp.com/).
  7. Wonder if you missed something, because that seems too easy.

This is the source code of the app:
```
daemon off;
worker_processes 1;
events { worker_connections 1024; }
error_log stderr;

http {
  server {
    default_type text/html;
    include env/heroku-http-port;
    access_log off;

    location / {
      content_by_lua '
        ngx.print("<h1>Hello from Lua!</h1>")
      ';
    }

  }
}
```

Here's what's going on line-by-line:
* `daemon off;`: this is important. `nginx` likes to run as a daemon, but apparently if a process daemonizes, Heroku assumes it has crashed. So, we ask `nginx` to not daemonize.
* `worker_processes 1;`: we want 1 `nginx` process. Pretty simple.
* `events { worker_connections 1024; }`: Tell you the truth, I don't know what this does. If you remove it, things crash. I assume it's the maximum number of connections each `nginx` process is supposed to be able to handle.
* `error_log stderr;`: Heroku will catch any errors printed here and you can see them on `heroku logs`
* `http {`: `nginx` can also be a mail server. This indicates that Web-server configuration follows.
* `server {`: It can also have more than one Web server (on different ports, for example), so this delimits the scope of a single "server".
* `default_type text/html;`: Unless otherwise specified, the server should tell the browser to expect HTML content.
* `include env/heroku-http-port;`: Heroku doesn't let apps run on port 80 directly; they're supposed to run on some other random port specified at runtime by the `$PORT` variable. Port 80 will get "routed" to them magically. My buildpack sets up this `env/heroku-http-port` file for you at runtime, which will say something like `listen 22310;`. It's important that not use a `listen` line (like `listen 80`) yourself.
* `access_log off;`: You can't access files like this on Heroku anyway, so may as well turn it off.
* `location / {`: This will match any request path starting with a `/` (and they all start with a `/`). `nginx`'s path matcher is super nice, and you should make good use of it: it'll choose the most specific match (so `location /foo` can coexist with `location /`), you can specify exact matches (`location = /`), and even Perl-compatible regex matches (`location ~ /(acceptable|directories|to|serve)/(.*)$`).
* `content_by_lua '`: `content_by_lua` is among the OpenResty extensions to `nginx`. It specifies some Lua code that's going to handle the request.
* `ngx.print("<h1>Hello from Lua!</h1>")`: This is pretty self-explanatory, but have a look at this [thorough documentation](http://wiki.nginx.org/HttpLuaModule) of the `ngx` API.
