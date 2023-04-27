You'll need an echo server (or whatever) running on port 8081:

```
docker run -d -p 8081:8081 ealen/echo-server --port 8081
```

Then start envoy:

```
envoy -c ./envoy.yaml
```

And make a request:

```
❯ curl http://localhost/test -v |  jq .
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 127.0.0.1:80...
* Connected to localhost (127.0.0.1) port 80 (#0)
> GET /test HTTP/1.1
> Host: localhost
> User-Agent: curl/7.87.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< content-type: application/json; charset=utf-8
< content-length: 657
< etag: W/"291-ADK3CLF79uDgDpzl0xm+3rJWeLw"
< date: Thu, 27 Apr 2023 21:47:38 GMT
< x-envoy-upstream-service-time: 5
< server: envoy
<
{ [657 bytes data]
100   657  100   657    0     0  48075      0 --:--:-- --:--:-- --:--:-- 93857
* Connection #0 to host localhost left intact
{
  "host": {
    "hostname": "localhost",
    "ip": "::ffff:172.17.0.1",
    "ips": []
  },
  "http": {
    "method": "GET",
    "baseUrl": "",
    "originalUrl": "/test",
    "protocol": "http"
  },
  "request": {
    "params": {
      "0": "/test"
    },
    "query": {},
    "cookies": {},
    "body": {},
    "headers": {
      "host": "localhost",
      "user-agent": "curl/7.87.0",
      "accept": "*/*",
      "x-forwarded-proto": "http",
      "x-request-id": "3daf48ff-4ac7-9ef4-be57-0f5711c8e9d9",
      "x-envoy-expected-rq-timeout-ms": "15000",
      "traceparent": "00-ae8e0ff4c0ce27fb01903b51f7775a91-c367a937134fdd3d-01"
    }
  },
  "environment": {
    "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    "HOSTNAME": "c8e286ba5e07",
    "NODE_VERSION": "16.16.0",
    "YARN_VERSION": "1.22.19",
    "HOME": "/root"
  }
}
```

Note that `traceparent` is still there. If you leave the `x-request-id` drop in the rds.yaml file, that header will drop, but the `traceparent` will remain. 
