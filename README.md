Openresty
=========

A minimal package utilizing openresty components that will function against the nginx-extras package provided by Ubuntu in Xenial (16.04).

This package was created to allow individuals unfamiliar with openresty to easily get started with NGINX and HashiCorp Consul dynamic upstreams. 

More information on openresty here:

https://openresty.org

#### Instructions

*Setup Build System*
```
sudo apt-get install ruby2.3 ruby2.3-dev build-essential
sudo gem install bundler
```
*Build Package*
```
cd <path to project>
rake
```

