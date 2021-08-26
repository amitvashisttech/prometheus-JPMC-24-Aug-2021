## Excute the script.

## Genrate the basic HTTP Auth Password with following command:
```
htpasswd -c /etc/nginx/.htpasswd admin

systectl restart nginx
```

## Now try to access the Nginx on Https. https://172.31.0.100/
