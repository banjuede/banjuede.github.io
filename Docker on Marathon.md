**1. docker 命令**
> `sudo docker run -d --restart always -p 80:80 -v /usr/compass:/opt/www/compass -v /mnt/datacanvas/aps/share/datasource:/usr/lib/aps/datasource -e front_host=192.168.1.70 -v /mnt/datacanvas/aps/share/avatar:/opt/www/compass/public/2.0.1/avatar --name aps_compasstest registry.aps.datacanvas.com:5000/zetimg/compass:2.0.3`  
> (挂载路径：前面是hostPath，后面是containerPath)

**2. docker on Marathon 的json文件**
> **BRIDGE MODE**  
> `{
  "id": "/docker-compass",
  "cmd": null,
  "cpus": 1,
  "mem": 128,
  "disk": 0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "volumes": [
      {
        "containerPath": "/opt/www/compass",
        "hostPath": "/usr/compass",
        "mode": "RW"
      },
      {
        "containerPath": "/usr/lib/aps/datasource",
        "hostPath": "/mnt/datacanvas/aps/share/datasource",
        "mode": "RW"
      },
      {
        "containerPath": "/opt/www/compass/public/2.0.1/avatar",
        "hostPath": "/mnt/datacanvas/aps/share/avatar",
        "mode": "RW"
      }
    ],
    "docker": {
      "image": "registry.aps.datacanvas.com:5000/zetimg/compass:2.0.3",
      "network": "BRIDGE",
      "privileged": false,
      "parameters": [
        {
          "key": "publish",
          "value": "80:80"
        }
      ],
      "forcePullImage": false
    }
  },
  "env": {
    "front_host": "10.0.0.28"
  }
}`

> **HOST MODE**  
> `{
  "id": "docker-compass2",
  "cmd": null,
  "cpus": 1,
  "mem": 128,
  "disk": 0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "registry.aps.datacanvas.com:5000/zetimg/compass:2.0.3",
      "network": "HOST",
      "ports": [
        80
      ],
      "parameters": [],
      "forcePullImage": true
    },
    "volumes": [
      {
        "hostPath": "/usr/compass",
        "containerPath": "/opt/www/compass",
        "mode": "RW"
      },
      {
        "hostPath": "/mnt/datacanvas/aps/share/datasource",
        "containerPath": "/usr/lib/aps/datasource",
        "mode": "RW"
      },
      {
        "hostPath": "/mnt/datacanvas/aps/share/avatar",
        "containerPath": "/opt/www/compass/public/2.0.1/avatar",
        "mode": "RW"
      }
    ]
  },
  "env": {
    "front_host": "10.0.0.28"
  },
  "labels": {},
  "healthChecks": []
}`

> **Note:**  
> `registry.aps.datacanvas.com:5000/zetimg/compass:2.0.3`  
> Marathon会先从本地查找image,如果本地不存在就会根据image的路径去registry中pull，如果registry不需要认证就可以直接pull; 如果registry需要认证具体步骤见[官网](http://mesosphere.github.io/marathon/docs/native-docker-private-registry.html);

**3. 使用Marathon的Rest API提交json创建应用**
> `curl -X POST http://base1.zetyun.com/v2/apps -d @docker-compass.json -H "Content-type: application/json"`

**4. 使用Rest API查看应用信息**
> `http://mesosphere.github.io/marathon/docs/generated/api.html`  
> 信息够详细了就不解释了