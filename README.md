This repository contains all the source files needed to follow the series [Kubernetes and everything else](https://rinormaloku.com/series/kubernetes-and-everything-else/)

This is the best introduction to Kubernetes and Everything related to be able to deploy scalable and resilient applications on Kubernetes managed clusters.


frontend部署到kubernetes时，nginx配置文件中做转发以解决跨域问题:

App.js中访问webapp的API调用, 写成/api/sentiment, 在nginx配置中，将/api开头的url全部重写为去掉/api并转发到其他服务，如访问nginx的/api/sentiment请求会被转发到http://sa-web-app-lb/sentiment. 而sa-web-app-lb:80即webapp在kubernetes的service名称和端口。



```javascript
analyzeSentence() {
        fetch('/api/sentiment', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({sentence: this.textField.getValue()})
        })
            .then(response => response.json())
            .then(data => this.setState(data));
    }
```

```
    location /api/ {
        rewrite ^.+api/?(.*)$ /$1 break;
        include uwsgi_params;
        proxy_pass http://sa-web-app-lb/;
    }
```
