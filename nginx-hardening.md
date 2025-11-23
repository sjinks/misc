## Disable Access to nginx Configuration

```sh
dpkg-statoverride --update --add root root 0700 /etc/nginx
setfacl -m u:www-data:rx /etc/nginx
```
