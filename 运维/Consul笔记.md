## Consul基本用法

### 在路径上输入cmd，

####  打开 命令提示符 输入

1）开发模式。-dev 数据会基于内存，重启后注册的服务都会消失。
consul.exe agent -dev；启动consul
2）生产模式。-client -server 数据会持久化
consul.exe agent -client -server；启动consul

### 持久化启动方式：

call consul agent -server -bootstrap-expect=1 -ui -bind=127.0.0.1 -client=0.0.0.0 -data-dir=D:/appdata/Consul/data/ -config-dir=D:/appdata/Consul/dir/



call consul agent -server -bootstrap-expect=1 -ui -bind=127.0.0.1 -client=0.0.0.0 -data-dir=D:/CastcZhy.Products/1.consul/config/ -config-dir=D:/CastcZhy.Products/1.consul/config/dir/


### 访问路径：

http://127.0.0.1:8500/ui/dc1/kv/Consul/



### 查看网卡信息

ipconfig 

#-dev -bind  设定绑定网卡，直接就启动服务了
#通过agent命令，来启动consul 服务


### key/value 导出

consul kv export --http-addr=http://127.0.0.1:8500  >consul_kv.json

### key/value 导入

consul kv import --http-addr=http://127.0.0.1:8500 @consul_kv.json



## Consul开启登录ACL

### 步骤1：添加consul配置文件，开启ACL。

创建config/dir目录，将以下配置文件放在此目录下，命名为acl.json

```json
{
    "acl": {
        "enabled": true,        //是否开启ACLToken
        "default_policy": "deny",  //allow和deny两个值。allow模式下，ACL是黑名单，允许任何未明确禁止的操作。deny模式下，ACL是白名单，阻止任何未明确允许的操作。
		"enable_token_persistence": true //值为true时，API使用的令牌集合将被保存到磁盘，并且当代理重新启动时会重新加载。
    }
}
```



```sh
启动consul

启动命令：

consul.exe agent -server -bootstrap -advertise 127.0.0.1 -data-dir ./data -ui -config-dir ./config/dir/acl.json
```



### 步骤2：获取bootstrap token

```sh
consul acl bootstrap
```

```sh
AccessorID:       b561dae8-0ec5-bdc5-2ed6-28d4350ca82d
# token的内容
SecretID:         ea9f39c6-8e4c-e8ed-30fb-6accff325ad3
#描述
Description:      Bootstrap Token (Global Management)
Local:            false
Create Time:      2025-01-23 15:00:08.9862145 +0800 CST
# 使用的全局策略，权限很大，类似mysql的root
Policies:
   00000000-0000-0000-0000-000000000001 - global-management
```

### 步骤3：代码修改

#### Program.cs

```c#
if (oConsulConfigurationAppsetting != null)
{
    foreach (var itemSetting in oConsulConfigurationAppsetting.DictionaySettingFiles)
    {
        builder.Configuration.AddConsul(itemSetting.Value, op =>
        {
            op.ConsulConfigurationOptions = cco =>
            {
                cco.Address = new Uri(oConsulConfigurationAppsetting.Server);
                //添加Token
                cco.Token = "ea9f39c6-8e4c-e8ed-30fb-6accff325ad3";
            };
            op.ReloadOnChange = true;
        });
    }
}
```

### ConsulConfig.cs

```c#
ConsulClient client = new ConsulClient(c =>
{
    c.Address = new Uri(config.Address);
    c.Datacenter = config.DataCenter;
    //添加Token
    c.Token = "ea9f39c6-8e4c-e8ed-30fb-6accff325ad3";
});
```



## Windows Consul 1.21.1集群 

在Windows环境中配置Consul集群主要涉及以下几个步骤：

### 1. 安装Consul

首先，你需要在所有参与集群的Windows服务器上安装Consul。你可以从[HashiCorp官网](https://www.hashicorp.com/products/consul)下载Consul的安装包。

#### 安装步骤：

1. 下载Consul的Windows安装包。
2. 解压下载的文件。
3. 打开命令提示符（CMD）或PowerShell，切换到解压后的目录。
4. 运行`consul.exe agent -dev`来启动一个开发模式的Consul代理，这适用于测试和开发环境。对于生产环境，你应该使用`-server`选项来启动服务器节点。

### 2. 配置Consul集群

#### 服务器节点配置：

1. 在每个服务器节点上，打开`consul.exe`的配置文件（通常是`consul.json`或通过命令行参数指定）。
2. 设置`server`选项为`true`以指定该节点为服务器节点。
3. 添加或指定`bootstrap_expect`值，该值应等于预期的服务器节点总数。例如，如果你有3个服务器节点，设置`"bootstrap_expect": 3`。
4. 配置数据中心名称（`datacenter`），所有节点应属于同一数据中心。
5. 配置集群的其他必要设置，如日志级别、绑定地址等。

示例配置（`consul.json`）: D:\consul_1.21.1_windows_amd64\config|cousul.json

```json
{
  "server": true,
  "bootstrap_expect": 3,
  "node_name": "consul-node-1",
  "datacenter": "dc1",
  "log_level": "WARN",
  "disable_update_check": true,
  "data_dir": "D:\\consul_1.21.1_windows_amd64\\config\\data",
  "client_addr": "0.0.0.0",
  "retry_join": ["10.128.35.136","10.128.35.137","10.128.35.138"],
  "bind_addr": "10.128.35.136",  
  "ui_config": {
	  "enabled": true
  },
  "acl": {
    "enabled": true,
    "default_policy": "deny",  
    "enable_token_persistence": true,  
    "down_policy": "extend-cache",  
    "tokens": {
      "initial_management": "ad732a8d-696a-f411-1dd0-0fb8173cf3f6",
	  "agent": "ad732a8d-696a-f411-1dd0-0fb8173cf3f6"  
    }
  }
}
```

```json
{
  "server": true,
  "bootstrap_expect": 3,
  "node_name": "consul-node-2",
  "datacenter": "dc1",
  "log_level": "WARN",
  "disable_update_check": true,
  "data_dir": "D:\\consul_1.21.1_windows_amd64\\config\\data",
  "client_addr": "0.0.0.0",
  "retry_join": ["10.128.35.136","10.128.35.137","10.128.35.138"],
  "bind_addr": "10.128.35.137",  
  "ui_config": {
	  "enabled": true
  },
  "acl": {
    "enabled": true,
    "default_policy": "deny",  
    "enable_token_persistence": true,  
    "down_policy": "extend-cache",  
    "tokens": {
      "initial_management": "ad732a8d-696a-f411-1dd0-0fb8173cf3f6",
	  "agent": "ad732a8d-696a-f411-1dd0-0fb8173cf3f6"  
    }
  }
}
```

```json
{
  "server": true,
  "bootstrap_expect": 3,
  "node_name": "consul-node-3",
  "datacenter": "dc1",
  "log_level": "WARN",
  "disable_update_check": true,
  "data_dir": "D:\\consul_1.21.1_windows_amd64\\config\\data",
  "client_addr": "0.0.0.0",
  "retry_join": ["10.128.35.136","10.128.35.137","10.128.35.138"],
  "bind_addr": "10.128.35.138",  
  "ui_config": {
	  "enabled": false
  },
  "acl": {
    "enabled": true,
    "default_policy": "deny",  
    "enable_token_persistence": true,  
    "down_policy": "extend-cache",  
    "tokens": {
      "initial_management": "ad732a8d-696a-f411-1dd0-0fb8173cf3f6",
	  "agent": "ad732a8d-696a-f411-1dd0-0fb8173cf3f6"  
    }
  }
}
```




### 3. 启动Consul

在所有节点上，使用以下命令启动Consul代理：D:\consul_1.21.1_windows_amd64\consul.bat

```
call consul agent -config-dir=D:/consul_1.21.1_windows_amd64/config
pause
```

### 4. 验证集群状态

你可以通过运行以下命令来查看集群的状态：

```
http://10.128.35.136:8500 看到3个节点信息，并有leader信息等。
```

这将列出所有集群成员及其状态。

通过以上步骤，你可以在Windows环境中成功配置并运行Consul集群。确保网络设置允许节点间通信，特别是TCP和UDP端口（默认8300, 8301, 8302, 8400, 8500）。