# web
## traefik
```
http反向代理/负载均衡
https://github.com/traefik/traefik
```

## caddy
```
定位跟nginx差不多，支持自动https
https://github.com/caddyserver/caddy
```

## kratos
```
微服务框架
https://github.com/go-kratos/kratos
```



## goreplay
```
流量重放
https://github.com/buger/goreplay
```

## fabio
```
基于consul的负载均衡
https://github.com/fabiolb/fabio
```


# 图形
## caire
```
resize
它是如何工作的
根据提供的图像生成能量图（边缘检测）。
该算法尝试在考虑最低能量值的情况下找到图像中最不重要的部分。
使用动态编程方法，该算法将在图像上从上到下或从左到右（取决于水平或垂直调整大小）生成单个接缝，并为每个接缝分配一个自定义值，最不重要的像素具有最低能源成本和最重要的成本最高。
我们从第二行到最后一行遍历图像，并计算每个条目所有可能连接接缝的累积最小能量。
通过将当前像素值与从前一行获得的相邻像素的最小值相加来计算最小能量水平。
我们从上到下遍历图像并计算最小能量水平。对于一行中的每个像素，我们计算当前像素的能量加上其上方三个可能像素之一的能量。
从最后一行开始的能量矩阵中找到成本最低的接缝并将其删除。
重复这个过程。
https://github.com/esimov/caire
```
## imgproxy
```
图片代理服务器(支持resize等)
https://github.com/imgproxy/imgproxy
```

## imaging
```
https://github.com/disintegration/imaging
```

## imageproxy
```
图片代理服务器
https://github.com/willnorris/imageproxy
```

## pigo
```
人脸检测
https://github.com/esimov/pigo
```

# 存储
## monio
```
oss
https://github.com/minio/minio
```

# 缓存
## ristretto
```
https://github.com/dgraph-io/ristretto
```

# GUI


## fyne
```
gui框架
https://github.com/fyne-io/fyne
```

## gioui
```

https://gioui.org/#installation
```

# 定时任务

## corn 
```
https://github.com/robfig/cron
```

## gocron
```
https://github.com/jasonlvhit/gocron
```

# 多线程

## ants
```
ants 是一个高性能且低损耗的 goroutine 池。
https://github.com/panjf2000/ants
```

# 工具

## hugo
```
用来生成静态网站，比如官网主页等
https://github.com/gohugoio/hugo
```

## cli
```
用于构建cli客户端
https://github.com/urfave/cli
```

## harbor

```
docker私有仓库
https://github.com/goharbor/harbor
```

## cadvisor
```
容器诊断
https://github.com/google/cadvisor
```

## kind
```
k8s测试集群
https://github.com/kubernetes-sigs/kind
```

## soar
```
SQL优化
https://github.com/XiaoMi/soar
```

## nps
```
内网穿透
https://github.com/ehang-io/nps
```

## gopsutil
```
主机信息监控/采集
https://github.com/shirou/gopsutil
```

## fsnotify
```
本地文件变动监控
https://github.com/fsnotify/fsnotify
```

## decimal
```
https://github.com/shopspring/decimal
```

## heimdall
```
http客户端
https://github.com/gojek/heimdall
```
