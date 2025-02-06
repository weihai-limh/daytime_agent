## 数字孪生相关

<br>数字孪生相关的数据结构设计是基于ISO(IFC)和国标的逻辑.
<br>数字孪生相关业务的数据处理逻辑参考'基于bim的数据分析'

----------
### 应用逻辑
<br>我将数字孪生相关的数据分为相对固定的'场景资源'和可以随'时序'更新的'资产资源';
<br>'资产资源'中的'资产实例'即为现实中实际存在的对象;
<br>'场景资源'和'资产实例'通过'容器'挂接,'容器'也是类型为'容器'的'一种资产实例';
<br>这样'容器'即时IFC语义(ISO 16739-1:2018)下空间中的一种元素,也可以兼容其他类型的属性(例如:openusd);
<br>以上共同构成'数字孪生'的基础数据结构<br>
![场景资源和资产实例](https://github.com/weihai-limh/daytime_agent/blob/main/document/image/digital_twin_correlation_pic1.png )
### 数据构成
#### 场景资源
<br>场景资源通过'指令:语义转化'服务将'普通资源'转化为'场景资源'
<br>我构造转化服务,'普通资源'当前接受以下内容到'场景资源'的转化<br>
![场景资源转化和应用](https://github.com/weihai-limh/daytime_agent/blob/main/document/image/digital_twin_correlation_pic2.png )
<br>'场景资源'在具体的消费实践中通过'用户请求ip'及'用户请求问题'通过H3确认用户的'地理范围',
<br>并通过'地理范围'中获取'资源',并根据'用户请求问题'转化为对应领域的'场景资源',
<br>并按照'用户请求问题'继续调用对应'微服务'对'场景资源'进行后续处理
#### 资产资源
<br>资产资源的实例即为'资产实例','资产实例'的类对应现实中的一类物品
<br>资产实例如下
```
```

### 机械操作
#### 机器指令
机器示例
```
{
    "机械臂1":	//机器代称
    {
        "ip": "192.168.1.199",	//机器ip
        "build":"workshop",	//机器所在建筑
        "type": "weixue_arm"	//机器型号
    },
    "机器车1":
    {
        "ip": "192.168.1.211",
        "build":"workshop",
        "type": "weixue_car"
    }
}
```
配置
```
//weixue
{
    "car":
    {
        "机器车移动":
        {
            "code":
            {
                "T": 1
            },
            "described": "weixue",
            "parameters":
            {
                "X": 45,
                "Y": 45,
                "SX": 300,
                "SY": 300
            }
        },
        "车载云台转动":
        {
            "code":
            {
                "T": 134
            },
            "described": "weixue",
            "parameters":
            {
                "X": 45,
                "Y": 45,
                "SX": 300,
                "SY": 300
            }
        },
        "车载云台手动转动":
        {
            "code":
            {
                "T": 104
            },
            "described": "weixue",
            "parameters":
            {
                "X": 0,
                "Y": 0,
                "SPD": 300
            }
        },
        "控制车载LED":
        {
            "code":
            {
                "T": 132
            },
            "described": "weixue",
            "parameters":
            {
                "IO4": 255,
                "IO5": 255
            }
        },
        "控制车载相机":
        {
            "code":
            {
                "T": 999
            },
            "described": "weixue",
            "parameters":
            {
                "IO4": 255,
                "IO5": 255
            }
        }
    },
    "arm":
    {
        "机械臂复位":
        {
            "code":
            {
                "T": 100
            },
            "described": "weixue",
            "parameters": ""
        },
        "机械臂状态查询":
        {
            "code":
            {
                "T": 105
            },
            "described": "weixue",
            "parameters": ""
        },
        "坐标移动":
        {
            "code":
            {
                "T": 1041
            },
            "parameters":
            {
                "x": 235,
                "y": 0,
                "z": 234,
                "t": 3.14
            }
        },
        "单关节弧度控制":
        {
            "code":
            {
                "T": 101
            },
            "described": "weixue",
            "parameters":
            {
                "joint": 0,
                "rad": 0,
                "spd": 0,
                "acc": 10
            }
        },
        "全关节弧度控制":
        {
            "code":
            {
                "T": 122
            },
            "described": "weixue",
            "parameters":
            {
                "b": 0,
                "s": 0,
                "e": 90,
                "h": 180,
                "spd": 10,
                "acc": 10
            }
        },
        "新建工作流":
        {
            "code":
            {
                "T": 220
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "intro": "test mission created in flash."
            }
        },
        "根据名称读取工作流":
        {
            "code":
            {
                "T": 221
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a"
            }
        },
        "在工作流尾部插入指令":
        {
            "code":
            {
                "T": 222
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "step": ""
            }
        },
        "在工作流的指定位置插入指令":
        {
            "code":
            {
                "T": 225
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3,
                "step": ""
            }
        },
        "在工作流的指定位置替换指令":
        {
            "code":
            {
                "T": 228
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3,
                "step": ""
            }
        },
        "在工作流中添加一个延时指令":
        {
            "code":
            {
                "T": 224
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "delay": 3000
            }
        },
        "在工作流的指定位置插入一个延时指令":
        {
            "code":
            {
                "T": 227
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3,
                "spd": 3000
            }
        },
        "在工作流的指定位置替换一个延时指令":
        {
            "code":
            {
                "T": 230
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3,
                "delay": 3000
            }
        },
        "在工作流的指定位置删除一个指令":
        {
            "code":
            {
                "T": 231
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3
            }
        },
        "根据名称播放任务流":
        {
            "code":
            {
                "T": 242
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "times": 0
            }
        },
        "根据名称从指定位置开启执行循环":
        {
            "code":
            {
                "T": 223
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "spd": 0.25
            }
        },
        "根据名称从插入位置开启执行循环":
        {
            "code":
            {
                "T": 226
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3,
                "spd": 0.25
            }
        },
        "根据名称从替换位置开启执行循环":
        {
            "code":
            {
                "T": 229
            },
            "described": "weixue",
            "parameters":
            {
                "name": "mission_a",
                "stepNum": 3,
                "spd": 0.25
            }
        }
    }
}
```
