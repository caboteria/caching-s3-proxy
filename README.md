Caching S3 Proxy 
----

Provides an unauthenticated plain HTTP frontend 
for public and private S3 buckets, and caches on the filesystem.
Least Recently Used objects are evicted from the cache first.

Example:
```
  python setup.py install
  export AWS_ACCESS_KEY_ID=...
  export AWS_SECRET_ACCESS_KEY=...
  caching-s3-proxy &
  curl localhost:8000/my_bucket/v1/my_file.txt
```

If you want to listen on a different port, just set the PORT variable:
```
  PORT=9999 caching-s3-proxy
```

You can also set CAPACITY (in bytes) and CACHEDIR.  If you set
CAPACITY to 0 the cache will be disabled so every request to
caching-s3-proxy will cause a request to S3.

Alternatively, you can run under uwsgi. It's safe to use multiple workers
processes (the shared file cache uses file locking to allow concurrency):
```
  uwsgi -w proxy.wsgi --http=localhost:8000 --workers=10
```

If you want to put this behind Nginx or Apache, use a socket instead:
```
  uwsgi -w proxy.wsgi -s /var/run/caching-s3-proxy.sock --workers=10
```

Then see http://uwsgi-docs.readthedocs.org/en/latest/Nginx.html or
http://uwsgi-docs.readthedocs.org/en/latest/Apache.html
