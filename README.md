# psusememcachedvarnishspeeduplinuxwebapp
## 4. Caching Full Pages with Varnish
### 2 Demo: Installing and Using Varnish
```
apt-get install varnish
```
default config file
```
vim  /etc/varnish/default.vcl
```
edit
```
backend default{
  .host="127.0.0.1"
  .port="8080"  //change this to 80
}
```
```
sub vcl_recv{
  if(req.url~"^/top-posts"){
    unset req.http.Cookie;
  }
}
```
```
sub vcl_fetch{
  if(req.url~"^/top-posts"){
    unset beresp.http.Set-Cookie;
  }
}
```
then
```
service varnish restart
```

#### 3:56
```
vim /etc/apache2/ports.conf
```
change port to 8040. then
```
vim /etc/apache2/ports.conf
```
then change virtualhost
```
vim /etc/apache2/sites-enabled/000-default
```

change
```
vim /etc/default/varnish
```
edit
```
DAEMON_OPTS="-a :80 \
```
restart server
```
service apache2 restart
service varnish restart
```
when we visit the page we can remove :6081



### 5 Demo: Forcing Page Refresh
```
$curl = curl_init("http://beta");
curl+set_opt($curl,CURLOPT_CUSTOMREQUEST,"PURGE");
curl_exec($curl);
```


## 5. Caching Individual Page Elements with Varnish
### 1 Using Edge Side Includes
### 2 Demo: Creating an Edge Side Include
```
vim /etc/varnish/default.vcl
```
```
sub vcl_fetch {
  set beresp.grace = 30m;
  set beresp.do_esi = true;
}
```
then
```
service varnish restart
```
