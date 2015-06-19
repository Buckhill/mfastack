# Scaling

MFAStack consists of two parts - an HTTP frontend and database backend (`KSM`). 

Since MFAStack doesn't use server-side sessions nor does it rely on any type of software installed directly on the server where PHP runs, scaling MFAStack is straight forward.

The preferred way of running PHP is by utilizing `PHP-FPM` and using Nginx to pass the requests via FastCGI to PHP.

Due to this fact, one Nginx frontend web server can communicate with multiple `PHP-FPM` servers, also known as FCGI clusters. 

## Scaling the HTTP frontend

An example of Nginx configuration file to load-balance the requests between 5 dedicated machines running `PHP-FPM`:

```
http {
    upstream mymfastack {
        server 01.php-fpm.com;
        server 02.php-fpm.com;
        server 03.php-fpm.com;
        server 04.php-fpm.com;
        server 05.php-fpm.com;
    }

    server {
        listen 443;

        location / {
            proxy_pass http://mymfastack;
        }
    }
}
```

We're leaving system administrators to determine the method of load-balancing the HTTP frontend using their own expertise and knowledge. 

The fact MFAStack doesn't rely on any one server in particular is enough to distribute the load safely on multiple machines without issues.


## Scaling the KSM

MFAStack doesn't use any advanced database features such as triggers, views, stored procedures, scheduled events nor any specific SQL dialect.
This allows us to use any of the four supported vendors interchangeably (MySQL, Postgresql, SQLite, MS SQL) and DBAs with experience with any of them are free to implement their knowledge to scale any of the mentioned databases as they see fit.

However, a database such as MySQL running on reasonably fast server (quad core @ ~3ghz, 16GB of RAM, SSD permanent storage) is more than capable of handling tens of millions of records.
Database replication, clustering, read/write replicas are welcome solutions to scaling the `KSM`.