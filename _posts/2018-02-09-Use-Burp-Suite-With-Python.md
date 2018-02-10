---
published: true
layout: post
title: Proxy Python Requests Through Burp Suite
---
I write a lot of Python scripts that interact with websites using the requests module. To figure out the requests I need to make for say, logging in, I do the process manually while Burp Suite is running and then model it. It wasn't until a few days ago while debugging that I wondered if I could proxy my Python programs to make sure it was sending the correct data.

### Configuration
I have Burp Suite running for both HTTP and HTTPS on `localhost:8080`.

```python
import requests

# Initialize Session object
s = requests.Session()
s.proxies = {'http' : 'localhost:8080', 'https' : 'localhost:8080'}

r = s.get("https://httpbin.org/get", verify=False)
```

If `verify` is not set to False, it will exit with an SSLError since it can't verifty the certificate from Burp Suite.

Now you can see the exact data Python sends.

### Resources
[Requests Documentation](http://docs.python-requests.org/en/master/)
