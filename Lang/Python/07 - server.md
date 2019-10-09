## server
### server.py
```python
import sys
import BaseHTTPServer
from SimpleHTTPServer import SimpleHTTPRequestHandler
HandlerClass = SimpleHTTPRequestHandler
ServerClass  = BaseHTTPServer.HTTPServer
Protocol     = "HTTP/1.0"

if sys.argv[1:]:
    port = int(sys.argv[1])
else:
    port = 8000
server_address = ('0.0.0.0', port)

HandlerClass.protocol_version = Protocol
httpd = ServerClass(server_address, HandlerClass)

sa = httpd.socket.getsockname()
print "Serving HTTP on", sa[0], "port", sa[1], "..."
httpd.serve_forever()
```
### get_file_from_local.sh
```bash
#!/bin/bash

v_filename=$1
v_ip="xxx.xxx.xxx.xxx"
v_port="8000"
v_url="http://$v_ip:$v_port/$v_filename"

if [ "" != "$v_filename" ]; then
    wget -e "http_proxy=proxyIp:proxyPort" $v_url && exit
fi

echo "please input filename..."
```



