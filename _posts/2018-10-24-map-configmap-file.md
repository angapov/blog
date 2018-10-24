---
layout: post
title:  "Map particular file from ConfigMap to pod in Kubernetes"
date:   2018-10-24 12:52:00 +0530
tags: [kubernetes, configmap]
categories: [kubernetes]
---
If you want to map one particular file from ConfigMap without wiping out the whole destination directory - this is the proper [receipt][receipt]:
```yaml
        volumeMounts:
        - name: test
          mountPath: /etc/apache2/conf-enabled/test.conf
          subPath: test.conf
      volumes:
      - name: test
        configMap:
          name: sstest
```

In case you have more than one file in ConfigMap:

```yaml
        volumeMounts:
        - name: test
          mountPath: /etc/apache2/conf-enabled/test.conf
          subPath: test.conf
      volumes:
      - name: test
        configMap:
          name: test
          - key: test.conf
            path: test.conf
```

[receipt]: https://stackoverflow.com/questions/44325048/kubernetes-configmap-only-one-file
