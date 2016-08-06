+++
date = "2016-07-20T22:13:21+01:00"
title = "IE compatibility modes"

+++

In order to support legacy websites or systems, those expecting IE6 for example (yes there are still a lot!), IE has the concept of compatibility modes.  It is common within enterprises to ensure that sites running on the local domain run in compatibility mode as they would break if they didn't.  I say it's common...it is in organisations with legacy web based systems but it should be getting less common there as time moves on.

What happens when you want an internal site to be running at the latest supportable IE version though? How do you ensure that happens when IE sees it as being part of the internal domain and forces it into compatibility mode?

Well there are a number of ways.

#### Edit the web.config file
This is for IIS only but the same principle holds true for other web servers.  The idea is to add a header to the outgoing response, ensuring that the page is displayed in the latest version that the IE client supports.

For the most part, if you have access to the web server, you should do this.  It's guaranteed to work in all situations (maybe - I'm sure there's some odd edgecase out there somewhere).

```
<system.webServer>
    <httpProtocol>
        <customHeaders>
            <add name="X-UA-Compatible" value="IE=Edge" />
        </customHeaders>
    </httpProtocol>
</system.webServer>
```

#### Add a meta tag to the page
This is useful if you don't have access to the web server.  The only issue here is that it doesn't always work.  It really depends on whether IE sets compatibilty mode before or after the meta tags have been read.

You'd think this would be a standard order of things, but in my experience I've seen some work and others not on the same infrastructure.  Maybe I've just not looked at it properly.

```html
<meta http-equiv="x-ua-compatible" content="IE=Edge">
```

#### Use the HTML5 doctype
Realistically you should be doing this anwyay.  Ensure that the HTML5 doctype is present at the top of the page.

```html
<!DOCTYPE html>
```