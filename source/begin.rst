开始使用DTP
===========

概览
----

DTP给开发者提供了语义理解模块和对话管理模块的算法黑箱，开发者不需要了解构建对话系统的复杂模型，只要上传一定量的语料和定义系统所需要的各种特定参数和规则，就可以创建出一个用来完成特定意图功能的任务型对话系统。通过借助DTP提供的云服务，开发者无需亲自掌握NLP、AI等技术，只要基于DTP的会话API即可在多种终端（网站、移动APP、智能硬件）中构建自己的智能会话机器人界面。

基本概念
--------

为了更好地理解DTP，我们需要先了解几个基本概念：

-  机器人：一个机器人对应一个独立的对话系统，用来满足开发者某个特定场景下的业务需求，比如下例中将要提到的“出行机器人”。
-  意图：意图表示用户的目的，比如在下面将要介绍到的例子中，意图就是“订机票”。
-  语义槽：语义槽表示用户表达中的关键语义信息，比如在“帮我订张去北京的机票吧”这句话中“北京”就是一个“出发地”语义槽。
-  实体：语义槽来源于实体，实体是对语义槽的抽象表示，可以在不同的意图间复用。
-  动作：机器人的回复被抽象成了动作，比如“请问您要去哪？”这句话就是“出行机器人”的一个动作。
-  语料：为了让机器人能够理解人的表达方式，我们需要添加语料，机器人经过训练可以学会识别意图及语义槽。语料越多，机器人的理解能力越强。

创建第一个机器人
----------------

本小节将会用一个简单的例子来说明DTP的基本使用流程，不会涉及到高阶特性。

首先我们创建一个出行机器人，用途是帮助用户订机票：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-01.png
   :alt: begin-01

   begin-01

创建完机器人之后，系统会为机器人添加几个默认的实体，其中location和date会在该场景中用到，location表示地点，date表示时间。在本例中，我们首先需要用到“出发地”和“到达地”两个实体。我们发现，“出发地”和“到达地”所包含的词汇信息都相同，但语义信息不同，因此，我们会在location下创建两个子实体，“from”和“to”：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-02.png
   :alt: begin-02

   begin-02

接下来我们需要创建“舱位类型”实体，该实体为自定义实体，我们可以为它添加词表，以扩展机器人可以识别的值的种类。

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-03.png
   :alt: begin-03

   begin-03

上述例子都是从句子中抽出的实体，但在某些情况下，语义信息无法直接从句子的词汇中提取出来，这个时候我们就需要用到“布尔型实体”，比如在该场景中，“是否需要保险”就是一个布尔型实体，它的值只有两个“是”或“否”。DTP中提供了一个内置的布尔型实体“is”，因此我们只需要在“is”下面添加相应的子实体就可以了。

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-04.png
   :alt: begin-04

   begin-04

然后我们需要给机器人添加一些列的回复动作，转至“动作”页面并进行添加，最终的结果如下图所示：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-05.png
   :alt: begin-05

   begin-05

接下来我们在“出行机器人”中创建一个“订机票”的意图：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-06.png
   :alt: begin-06

   begin-06

之后我们创建“出发地”、“到达地”、“时间”、“舱位类型”和“是否需要保险”五个语义槽，它们分别对应刚才创建的五个实体，并且都设定成不可缺失，这代表着只有在这些语义槽都被机器人识别出时，本段对话才算结束，同时，当其中的某一个槽缺失时，机器人会发出询问，具体采用何种方式询问，开发者可以自行定义：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-07.png
   :alt: begin-07

   begin-07

接下来我们需要添加订机票场景下的各种对话样本，也就是语料，同时将其中的语义槽标注出来。开发者可以在点开语料对话框后，通过鼠标拖拽选中指定词汇进行标注，标注结果中的不同颜色代表不同语义槽，标注时有两点需要额外注意：

1、通常情况下，我们不需要考虑机器人上轮的回复就可以区分用户表达，比如用户说“从哈尔滨出发”时，机器人就可以判断“哈尔滨”是“出发地”。但当用户仅说“哈尔滨”时，如果不分析机器人的问题“请问您要从哪出发呢？”，机器人就无法区分“哈尔滨”是“出发地”还是“到达地”，因此我们在标注这段语料时，需要指定机器人的动作，也就是说，当机器人问“请问您要从哪出发？”时，“哈尔滨”就被标注成“出发地”，反之则为“到达地”，标注方式如下图所示：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-08.png
   :alt: begin-08

   begin-08

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-09.png
   :alt: begin-09

   begin-09

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-10.png
   :alt: begin-10

   begin-10

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-11.png
   :alt: begin-11

   begin-11

2、布尔型语义槽不需要在句子中进行标注，我们可以直接在“布尔型语义槽”框内进行选择，标注方式如下图所示：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-12.png
   :alt: begin-12

   begin-12

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-13.png
   :alt: begin-13

   begin-13

最终添加好的语料如下图所示：

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-14.png
   :alt: begin-14

   begin-14

最后，我们点击训练按钮，待训练任务完成后，即可在右侧的栏目中进行对话测试。

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-15.png
   :alt: begin-15

   begin-15

.. figure:: https://dtp.oss-cn-beijing.aliyuncs.com/images/begin/begin-16.png
   :alt: begin-16

   begin-16

机器人训练完成后，开发者便可通过API来调用对话系统，API会返回机器人回复及当前语义槽的抽取结果。开发者在获得这些信息后，便可结合自己的系统开发更为丰富的功能。
