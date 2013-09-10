rush-http-proxy
===============

rush-http-proxy is based on node-http-proxy, the extra features are:
* Run multiple host/port configurations on a single instance.
* Support reading SNI configurations from file and CRT bundle files.
* Run worker instances, by default start number of workers equivalent to CPU number.
* Watch for config changes and reload. Currently it is a bulk reload.

Future plans:
* More intelligent reload.
* Support redirect and other filters in the config.
* Better logging.
* Systemd support.

Usage
===============

`rush-http-proxy --config config.json`

Example config:

```
{
  "ports": {
    "80": {
        "router": {
            "code2flow.*": "127.0.0.1:8099",
            ".*": "127.0.0.1:8080"
        }
    },
    "443": {
        "router": {
            "code2flow.*": "127.0.0.1:9991",
            "service.myapp.com/downloads/.*": "127.0.0.1:10443",
            "service.myapp.com/uploads/.*": "127.0.0.1:15000",
            ".*": "127.0.0.1:4443"
        },
        "https": {
            "SNI": {
                ".*service.myapp.com": {
                    "key": "/etc/keys/myapp_com.key",
                    "cert": "/etc/keys/myapp_com.pem",
                    "ca": [
                        "/etc/keys/ca.pem",
                        "/etc/keys/sub.class1.server.ca.pem"
                    ]
                }
            },
            "key": "/etc/keys/star_code2flow_com.key",
            "cert": "/etc/keys/star_code2flow_com.pem",
            "ca": "/etc/keys/certum.crt"
        }
    }
  }
}
```

Each entry in the `ports` is the format that would be normally fed to `node-http-proxy`.
