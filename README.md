# Example repo / Bug demo

Function using the PHP template. It doesn't allow to return a content type. As a work around it's been set in the yml file:

```
functions:
  php7-json:
    lang: php7
    handler: ./php7-json
    image: spekulatius/php7-json:latest
    environment:
      write_debug: true
      content_type: application/json
```

But this doesn't work when data is posted. It ignores the environment config and uses the header instead.

## Behavior

```
peter@x1:/tmp/php7-json$ curl -i http://faas1:8080/function/php7-json
HTTP/1.1 200 OK
Content-Length: 11
Content-Type: application/json
Date: Mon, 07 Dec 2020 14:21:36 GMT
X-Duration-Seconds: 0.021615

{"data":""}


peter@x1:/tmp/php7-json$ curl -i http://faas1:8080/function/php7-json --data-ascii "test"
HTTP/1.1 200 OK
Content-Length: 15
Content-Type: application/x-www-form-urlencoded
Date: Mon, 07 Dec 2020 14:21:53 GMT
X-Duration-Seconds: 0.024064

{"data":"test"}


peter@x1:/tmp/php7-json$ curl -i http://faas1:8080/function/php7-json -d "test=test"
HTTP/1.1 200 OK
Content-Length: 20
Content-Type: application/x-www-form-urlencoded
Date: Mon, 07 Dec 2020 14:22:00 GMT
X-Duration-Seconds: 0.020691

{"data":"test=test"}
```

## Expected Behavior

All three requests should return `Content-Type: application/json` as a header.


## Context/Versions

Faasd instance recently started. Version was initial 0.9.5 and later upgraded to 0.9.8-4-g0d9c846.

```bash
21:38 $ faas-cli version --gateway faas1:8080
  ___                   _____           ____
 / _ \ _ __   ___ _ __ |  ___|_ _  __ _/ ___|
| | | | '_ \ / _ \ '_ \| |_ / _` |/ _` \___ \
| |_| | |_) |  __/ | | |  _| (_| | (_| |___) |
 \___/| .__/ \___|_| |_|_|  \__,_|\__,_|____/
      |_|

CLI:
 commit:  c9d284d0c5bd90415baf9130fdce55af8e5a506b
 version: 0.12.19

Gateway
 uri:     http://faas1:8080
 version: 0.19.1
 sha:     f612947df76faea0132415151f9765518f443123
 commit:  Remove deprecated Angular/material versions


Provider
 name:          faasd
 orchestration: containerd
 version:       0.9.8-4-g0d9c846
 sha:           0d9c846117cc5db7b224e990fc61575fd793ce1c
✔ /tmp/php7-json [master L|✔]
```
