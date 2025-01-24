# 项目概述
<br>The project is sharing part of the design of a running agent-based application,
<br> which leverages LLMs for formal logical reasoning, 
<br>microservices for transforming reasoning results into data processing outcomes, 
<br>DDD and DSL for structuring and expressing domain knowledge, 
<br>and digital_twin technologies (ifc+gis+iot) 
<br>for realizing the physical manifestations of these data processing results.

<br>项目分享一个已经运行的智能体应用的部分设计.
<br>项目通过结合DDD+DSL进行LLM的'形式化逻辑推理';
<br>通过LLM+微服务完成'形式化逻辑推理'的'推理结果'到'数据处理结果'的转化;
<br>最后通过ifc+gis+iot完成'数据处理结果'到'物理现实的落地'.
<br>最终实现'AI以人类能理解的方式自主使用相关工具解决现实世界的问题'

----------
<br>我在从事产品经理的工作前做了6年的轨道交通设计,
<br>通过十多年在不同体系及系统中的工作,我发现大多数的工作或需求:
<br>一部分人是通过语言分配给其他人做,其他人是通过操作工具解决对应问题.
<br>过去工作或需求的最终执行起作用的是肌肉力量,
<br>随着时代的发展,现在起作用的可以是微服务.
<br>所以我的应用的核心是把微服务当作自然语言中的谓语及宾语被LLM推理及驱动.
<br>为了具有现实价值,让LLM在推理过程中根据场景调用知识进而使用工具和资产外,
<br>还需要在数字孪生空间中进一步模拟推理结果并向用户展示结果.
<br>我在这个基础上结合根据智慧城市等项目的经验结合自身对智慧生活的需要,
<br>最后搭建了以ifc+gis+iot为核心的微服务,实现了自己所期望的内容.
<br>龙蛇之变,木雁之间.所以提炼自有服务中的内容做为项目分享一些过程。
## 模块构成
<br>项目主要由以下四个模块构成：
<br>1.代理模块：作为用户与系统的接口，负责接收用户输入的问题，并将其传递给后续模块进行处理。
<br>2.知识管理模块：存储和管理与问题相关的知识，包括领域知识、常识等，用于辅助LLM生成解决方案。
<br>3.方案应用模块：接收由LLM和知识管理模块生成的解决方案，并对其进行解析和逻辑抽取，然后分配执行次序。
<br>4.指令模块：根据方案应用模块提供的执行次序，调用相应的微服务完成子任务，并最终实现解决方案。
### 模块流程
![模块说明附图](./document/image/daytime_agent_di1.png )
<br>1.用户输入问题：用户通过代理模块输入问题。
<br>2.生成解决方案：LLM结合知识管理模块中的知识，生成基于问题和知识的解决方案。
<br>3.解析与逻辑抽取：方案应用模块接收解决方案，利用LLM和图数据库从中抽取逻辑，并分配执行次序。
<br>4.执行指令：指令模块根据执行次序，依次调用相应的微服务完成子任务。
<br>5.成果展示：通过WebGL等数据可视化技术，向用户展示解决方案的成果。

## 子模块概述

### 指令模块
<br>指令模块是通过结构化自然语言对微服务进行调用的一种方法,指令的输入输出均有自然语言的模态.
<br>指令模块通过post(/processapi)被调用,
<br>调用后通过指令处理函数将指令映射为请求参数,并提交到对应的服务执行.
<br>指令模块的返回除了支持文本形式的返回外,
<br>如需文本以外的数据返回,可以额外调用'渲染函数'向调用方提供更友好的数据渲染.
<br>同时指令模块中的指令既可以通过'方案应用模块'被llm调用也可以单独被服务集成
![指令模块附图](./document/image/daytime_agent_di2_instructions.png)
<br>当指令负载文件资源时,可以参考下图逻辑:
![指负载资源](./document/image/instructions_load_resources1.png)
#### 指令示例
<br>以下为我的指令部分指令说明及实现演示:
##### 空间类指令
<br>大多数活动与时空有关,由此通过以下指令完成对时空数据的消费.
<br>指令对空间数据的消费主要为对空间内对象的查询,模拟(碰撞检测,自动寻路,路径漫游...)
<br>从而获得对空间的分析结果,最终将结果做为其他指令的入参或通过图形渲染向用户渲染结果.
```
指令:场景推理;建筑计算,<空间名称>,<建筑计算子类型>,<建筑计算子参数>
	-用配置的建筑计算服务对<空间名称>对应的语义化数据按照<建筑计算子类型>,<建筑计算子参数>进行计算
指令:场景推理;建筑推理,<空间名称>,<建筑领域内要推理的问题>
	-用配置的建筑推理服务对<空间名称>对应的映射为建筑类语义化的数据按'LLM+建筑prompt'进行推理
指令:场景推理;导航,...

指令:场景推理;物品查找,...
地图导航类指令...
```
![建筑推理和建筑计算](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/space_process1.gif)
<br>以上示例为通过自然语言指令进行建筑推理,建筑计算及数据可视化
![室外导航数据生成及数据可视化](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/gisnav.gif)
<br>以上示例为通过自然语言指令进行室内导航数据生成及数据可视化
![室内导航数据生成及数据可视化](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/bimnav.gif)
<br>以上示例为通过自然语言指令进行室内导航数据生成及数据可视化
<br>(指令通过和HomeAssistant结合,亦可构建自己的智能家庭)
<br>我对于空间的理解主要分为室内和室外.空间数据的最低标准为其语义信息中具有经纬度信息.
<br>室内室外的空间转化通过uber的h3进行换算,当有可视化需求时室外通过webgis渲染,室内通过webgl渲染.
<br>室内数据主要为ifc为主的bim数据及openusd,室外数据主要为84的geojson
<br>室外空间对应的微服务建议封装为在线地图结服务合私有地图数据的形式.
<br>室外空间分析的逻辑为将数据同步为geojson后提交到后台通过QGIS进行分析
<br>室内空间的分析则根据问题的类型来判断,如果问题仅是模拟(碰撞检测,自动寻路,路径漫游...)可以通过图形引擎自身的物理能力进行模拟.
<br>如果问题包含空间的推理则可根据问题的复杂程度选择不同的处理模式.
<br>当出于'空间推理'模式时,会将当前场景中的所有实体对应的语义信息格式化为prompt经llm推理返回出结论.
<br>当出于'空间计算'模式时,会通过云服务进行对应的分析处理
<br>详见空间语义描述
##### 资源类指令
<br>资源类指令主要解决在语境中获得物理资源的问题;
<br>该指令通过部署的资源服务通过oss从NAS中获取提取对应资源

```
指令:基础应用;语义转化,workshop,bim

指令:基础应用;获取资源,workshop,space
```
![资源查找](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/access_resources.gif)

##### 数据处理类指令

<br>数据处理类指令通过封装资数据处理方向的微服务对文本中涉及的数据进行语义及模态的转化
<br>该类指令主要有三个方向,分别为:语义数据转化,影像数据处理,其他数据处理
<br>
<br>语义数据转化主要完成不同语义描述间的映射,
<br>例如同样由若干几何构件组成的场景后续若要在建筑运维语境下则将其转化为'bim';
<br>若要在数字孪生语境下应用则将其转化为'space';

```
指令:基础应用;语义转化,workshop,bim

指令:基础应用;获取资源,workshop,space
```
<br>影像数据处理主要处理音视频等影像资产,
<br>例如通过封装FFmpeg的微服务完成音视频的剪辑与提取;
<br>或者通过封装SAM和DepthPro的微服务完成图片中的对象提,追踪及点云重建;
<br>或者通过封装StableDiffusion的微服务将以上生成的素材重组为新的影像资产
```
指令:影像处理;遮罩生成,<url>,<遮罩类型>,目标对象
	-用部署的遮罩服务根据遮罩类型和参数处理url指向的图像或视频
指令:影像处理;深度转换,<url>,<输出类型>
	-用部署的深度转换服务根据输出类型处理url指向的图像
指令:影像处理;点云转换,<url>,<输出类型>
	-用部署的点云转换服务根据输出类型处理url指向的图像
```
<br>以上示例为通过自然语言指令调用图像处理指令完成对图像的处理
<br>其余数据处理归类于其他数据处理,
<br>并包含图表生成等基础数据可视化服务
```
指令:绘制图表;<图表类型>,<制表参数>
	-用配置的图表服务根据制表参数生成对应的图表
```
![绘制图表1](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/chart1.gif)
##### 在线服务集成指令
<br>在线服务集成指令通过将开放平台api的再次封装为微服务获得预期的数据

```

指令:基础ai;图像推理,图里有什么,<url>
	-用配置的多模态推理服务推理url指向的图片
指令:基础应用;语音合成,默认,<待合成的文本>
	-用配置的语音合成服务进行语音合成
指令:天气指数;天气查询,<时间>,<城市>
	-用配置的天气查询服务进行天气查询
指令:基础应用;语言转化,<待翻译的文本>,<转化类型>
	-用配置的翻译服务进行对应的翻译
指令:基础应用;发送邮件,<收件人>,<邮件标题>,<邮件内容>
	-用配置的邮件服务进行语音合成
```
![文本推理及图像推理](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/llm_chat_api.gif)
<br>以上示例为通过自然语言指令调用在线api完成指令内涉及的文本推理及图像推理
![语言识别](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/speechrecognition.gif)
<br>以上示例为通过自然语言指令完成语音识别及语音合成
![天气查询](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/weather.gif)
<br>以上示例为通过自然语言指令完成指令内的天气查询及各类天气相关的标签
![多语言翻译](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/translation.gif)
<br>以上示例为通过自然语言指令完成指令内的多语言翻译
### 方案应用模块
方案应用模块可对符合预期范式的文本段落进行推理.
推理分为线性推理和节点推理,线性推理通过正则获取文本中的逻辑,
节点推理通过antlr4+neo4j
#### 方案文本示例
![线性推理示例1](https://github.com/weihai-limh/daytime_agent/blob/main/document/video/linear_reasoning.gif)
<br>以上示例为线性推理示例
### 代理模块
#### 模块示例
##### 穿什么
