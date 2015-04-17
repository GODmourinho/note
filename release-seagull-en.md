
Seagull the friendly Web UI to monitor docker daemon

## Background

Everyone wants Web UI. This's the vision of seagull. Nowadays docker becomes more and more popular and lots of developers're trying to learn about docker. We have communicated with most of them. Then we noticed that the most concerned question for them is which tool to monitor docker is the recommanded. Actually, there're some open source projects to do that but they're barely unsatisfactory.

Then we hope the seagull team could develop a tool with friendly Web UI to monitor and manage docker. After a week of development, we released the first version and more and more developers have paid attention to it. Moreover, it bumped up to 100+ stars in Github within two weeks. Now it's the third week of seagull and we have more and more features for docker developers.

## Introduction

Seagull is the Web UI to monitor and manage docker daemon. In fact, it's a dockerized application as well. You can install and run seagull with `docker run -d -p 10086:10086 -v /var/run/docker.sock:/var/run/docker.sock tobegit3hub/seagull`. Then go to <http://127.0.0.1:10086> in your browser and you can monitor your images and containers of docker.

![](https://raw.github.com/tobegit3hub/seagull/master/screenshot.png)

To know more about seagull, here's a three-minute presentation, <https://www.youtube.com/watch?v=0BAiSx7l7Y4>. If you want to try seagull immediately, we provide a demo server in <http://96.126.127.93:10086>.

## Implement

Seagull is a light-weight single page application. It gets data from docker remote API by accessing unix socket. We do a lot to implement the front-end framework and support multiple languages. If you're interested in the details of seagull, you can think Seagull = Docker + Beego + AngularJS + Godep + Bower + Bootstrap + JQuery.Gritter + Animate.css.

We open source seagull in [Github](https://github.com/tobegit3hub/seagull) from the first commit. The best part is that we have introduced all the details of seagull in both English and Chinese. You can pick up the interested part to learn.

* [Access docker remote API](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-12-access-docker-remote-api.md)
* [Seagull design and implement](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-14-seagull-design-and-implement.md)
* [Implement i18n with angular translate](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-18-implement-i18n-with-angular-translate.md)
* [Seagull dockerfile](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-20-seagull-dockerfile.md)
* [Use godep to manage dependency](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-21-use-godep-to-manage-dependency.md)
* [Implement search in angular](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-22-implement-search-in-angular.md)
* [Use beego as web server](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-23-use-beego-as-web-server.md)
* [Use beego as api server](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-24-use-beego-as-api-server.md)
* [Use angular](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-25-use-angular.md)
* [Use bower to manage dependency](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-26-use-bower-to-manage-dependency.md)
* [How seagull use bootstrap](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-27-how-seagull-use-bootstrap.md)
* [Use jquery.gritter for notification](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-28-use-jquerygritter-for-notification.md)
* [Use animate.css](2014-10-30-use-animate-css.md)

## Roadmap

After releasing seagull 1.0, more and more developers're asking for their requirements. Seagull will try to implement more freatures in the future. Our goal is to provide the best tool to monitor and manage docker for developers.

From now on, we will focus on the monitor of container resources. We also want a web console to run docker commands in the browser. And sooner or later we will support French and German as well.

Feel free to send issues or pull-requests to seagull in <https://github.com/tobegit3hub/seagull>. Any suggestion is welcome!

