- to start
`sudo systemctl start nginx`
- to stop
`sudo nginx -s quit` - smooth stop
`sudo nginx -s stop` - fast stop

u can use kill and pkill utilities
- `sudo kill -s QUIT {pid of nginx}` # you can find pid in /run/nginx.pid
- `sudo pkill -f nginx & wait $!`