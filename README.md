# 项目概述
项目通过自然语言连接LLM和微服务，实现AI以人类能理解的方式使用特定工具来解决问题。
<br>我在从事产品经理的工作前做了6年的轨道交通设计,如果说工作内容有什么相通之处,
<br>那就是都是调动可以调动的资源来实现领导或更高层领导的意图。
<br>通过这些工作我意识到了人和人的差别在于言语指令是否会被执行和执行所引发的开支,
<br>一切自上而下的规划都要自下而上的实现,过去是肌肉劳动而现在则是微服务。
<br>过去建设工程的经验让我可以做一些数字孪生相关的微服务,并通过这些服务沟通虚实.
<br>基于此我通过构式句法整合微服务的调用,通过llm的加持让我的语言指令具有现实力量。
<br>龙蛇之变,木雁之间.所以提炼自有服务中的内容做为项目分享一些过程。
<br>项目主要由以下四个模块构成：
<br>1.代理模块：作为用户与系统的接口，负责接收用户输入的问题，并将其传递给后续模块进行处理。
<br>2.知识管理模块：存储和管理与问题相关的知识，包括领域知识、常识等，用于辅助LLM生成解决方案。
<br>3.方案应用模块：接收由LLM和知识管理模块生成的解决方案，并对其进行解析和逻辑抽取，然后分配执行次序。
<br>4.指令模块：根据方案应用模块提供的执行次序，调用相应的微服务完成子任务，并最终实现解决方案。
# 工作流程
![本地路径](./document/image/daytime_agent_di1.png "模块说明附图")
<br>1.用户输入问题：用户通过代理模块输入问题。
<br>2.生成解决方案：LLM结合知识管理模块中的知识，生成基于问题和知识的解决方案。
<br>3.解析与逻辑抽取：方案应用模块接收解决方案，利用LLM和图数据库从中抽取逻辑，并分配执行次序。
<br>4.执行指令：指令模块根据执行次序，依次调用相应的微服务完成子任务。
<br>5.成果展示：通过WebGL等数据可视化技术，向用户展示解决方案的成果。

## 子模块

### 指令模块
<br>指令模块是通过结构化自然语言对微服务进行调用的一种方法,指令的输入输出均有自然语言的模态.
<br>指令模块通过(/processapi)被调用,调用后通过activation_order函数将指令映射为请求参数,并提交到对应的服务执行.
<br>指令模块的返回除了支持文本形式的返回外,还支持音视频,模型等形式等形式的返回.
<br>音视频模型等相较于文本更易理解,调用时可以额外调用'渲染函数'向调用方提供更友好的数据渲染.
![本地路径](./document/image/daytime_agent_di2_instructions.png "指令模块附图")
#### 指令示例
<br>以下为我的指令部分指令说明及实现演示:
##### 空间类指令
<br>大多数活动与时空有关,由此通过以下指令完成对时空数据的消费.
<br>指令对空间数据的消费主要为对空间内对象的查询,模拟(碰撞检测,自动寻路,路径漫游...)
<br>从而获得对空间的分析结果,最终将结果做为其他指令的入参或通过图形渲染向用户渲染结果.
```
指令:场景推理;建筑计算,savs07,基础查询,楼层,F7

指令:场景推理;建筑推理,savs07,F2有哪些房间

指令:场景推理;导航,...

指令:场景推理;物品查找,...
地图导航类指令...
```
<br>建筑推理gif占位
<br>建筑计算gif占位
<br>我对于空间的理解主要分为室内和室外.空间数据的最低标准为其语义信息中具有经纬度信息.
<br>室内室外的空间转化通过uber的h3进行换算,当有可视化需求时室外通过webgis渲染,室内通过webgl渲染.
<br>室内数据主要为ifc为主的bim数据及openusd,室外数据主要为84的geojson
<br>室外空间对应的微服务建议封装为在线地图结服务合私有地图数据的形式.
<br>室外空间分析的逻辑为将数据同步为geojson后提交到后台通过QGIS进行分析
<br>室内空间的分析则根据问题的类型来判断,如果问题仅是模拟(碰撞检测,自动寻路,路径漫游...)可以通过图形引擎自身的物理能力进行模拟.
<br>如果问题包含空间的推理则可根据问题的复杂程度选择不同的处理模式.
<br>当出于'空间推理'模式时,会将当前场景中的所有实体对应的语义信息格式化为prompt经llm推理返回出结论.
<br>当出于'空间计算'模式时,会通过云服务进行对应的分析处理

##### 资源类指令
指令中涉及一些资产与数据的引用,由此通过以下指令完成对资源的引用
<br>该指令通过部署的资源服务通过oss从NAS中获取提取对应资源
##### 数据处理类指令
资源不是凭空产生的,由此通过以下指令完成资源语义及模态的转化
##### 在线服务集成指令
如正常的生活需要协作一样,指令也可以是对在线服务的集成

```

指令:基础ai;图像推理,图里有什么,<url>
	-用配置的多模态推理服务推理url指向的图片
指令:基础应用;语言转化,<待翻译的文本>,<转化类型>
	-用配置的翻译服务进行对应的翻译
指令:天气指数;天气查询,<时间>,<城市>
	-用配置的天气查询服务进行天气查询
指令:基础应用;语音合成,默认,<待合成的文本>
	-用配置的语音合成服务进行语音合成
指令:基础应用;发送邮件,<收件人>,<邮件标题>,<邮件内容>
	-用配置的邮件服务进行语音合成
```
<br>文本推理及视频推理
![本地路径](./document/video/llm_chat_api.mp4 "文本推理及视频推理")
<br>语言转化gif占位
<br>天气查询gif占位
<br>语音合成gif占位
### 方案应用模块
方案应用模块可对符合预期范式的文本段落进行推理.
推理分为线性推理和节点推理,线性推理通过正则获取文本中的逻辑,
节点推理通过antlr4+neo4j
#### 方案文本示例
<br>线性推理gif占位