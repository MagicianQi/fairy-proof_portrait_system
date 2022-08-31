# 画像标签体系建设

用户(项目)画像是指根据用户(项目)的属性、偏好、习惯、行为等信息而抽象出来的标签化用户(项目)的模型。
通俗说就是给用户(项目)打标签，而标签是通过对用户(项目)信息分析而来的高度精炼的特征标识。
通过打标签可以利用一些高度概括、容易理解的特征来描述用户(项目)，可以让人更容易理解用户(项目)，并且可以方便计算机处理。

我们主要是建立EOA地址画像、合约地址画像和项目的画像，为了方便后续区分不同地址和不同相同项目，并且给后续数据挖掘提供数据支持。


## 画像的作用

在互联网领域画像常用来作为精准营销、用户统计、推荐系统、数据挖掘等基础性工作。
但是对于我们，主要利用画像来实现对EOA地址、合约地址和项目的分析、统计等工作：

1. 用户(项目)统计：
   1. 根据用户(项目)的属性、行为特征对用户(项目)进行分类后，统计不同特征下的用户(项目)数量、分布。
   2. 分析不同用户(项目)画像群体的分布特征。
2. 数据挖掘：以用户(项目)画像为基础构建推荐系统，进行风险分析等。
3. 行业报告与用户研究：通过用户(项目)画像分析可以了解行业动态，比如根据项目画像输出分析报告等。


## 画像所需数据

一般来说，根据具体的业务内容，会有不同的数据，不同的业务目标，所以也会实现不同的画像。
在互联网领域，用户画像数据一般包括以下内容： 人口属性、兴趣特征、消费特征、位置特征、设备属性、行为数据、社交数据等等。

但是针对我们的EOA地址画像、合约画像和项目画像，数据的来源一般会来自两个方向：
1. 链上数据：记录在区块链上的数据，包括区块、交易、消息日志等。上链意味着共识和存储，数据则不可篡改。
2. 链下数据：除了区块链上的数据，例如项目的twitter数据、新闻数据等等。

我们需要结合链上数据与链下数据一起分析得到画像标签。

## 构建画像的基本步骤

画像最重要的一个步骤就是对用户(项目)标签化，我们要明确要分析用户(项目)的各种维度，才能确定如何对用户(项目)进行画像。

在建立用户画像上，可以分为以下几个步骤：
1. 基础数据收集：包括上一节中描述的链上数据与链下数据。
2. 画像体系架构：设计画像的体系架构，包括画像层级、画像依赖、画像类型、画像主题等设计。
3. 数据分析加工：当我们对用户(项目)画像所需要的基础数据收集完毕后，需要对这些资料进行分析和加工，提炼关键要素，构建可视化模型。
   对收集到的数据进行统计、规则抽取和模型设计等，抽象出用户(项目)的各类标签
4. 画像标签系统架构：利用大数据的整体架构对标签化的过程进行开发实现，对数据进行加工，将标签管理化。
   同时将标签计算的结果进行计算。这个过程中需要依靠Hive，Hbase等大数据技术，为了提高数据的实时性，还要用到Flink，Kafka等实时计算技术。 
5. 标签可视化：要将我们的计算结果，数据，接口等等，形成服务。比如，图表展示，可视化展示等等。

## 画像的系统架构(仅供参考)

画像标签系统为了方便使用和保持鲁棒性，一般需要保证大数据、实时性和可视化等特性。

主要实现的挑战分为以下几个方面：
1. 大数据: 整体用户(项目)画像体系必须建立在大数据架构之上。
2. 实时性: Kafka和Flink等实时流式计算框架的数据的一致性，事件时间窗口，水印，触发器都成为很容易的实现。而实时的OLAP框架Druid更是让交互式实时查询成为可能。
3. 数据仓库: Hive是作为离线数仓的不二选择，而hive使用的新引擎tez也有着非常好的查询性能，而最近新版本的Flink也支持了hive性能非常不错。但是在实时用户画像架构中，Hive是作为一个按天的归档仓库的存在，作为历史数据形成的最终存储所在，也提供了历史数据查询的能力。而Druid作为性能良好的实时数仓，将共同提供数据仓库的查询与分析支撑，Druid与Flink配合共同提供实时的处理结果，实时计算不再是只作为实时数据接入的部分，而真正的挑起大梁。

4. 依据上面的分析与我们要实现的功能，我们将依赖Hive和Druid建立我们的数据仓库，使用Kafka进行数据的接入，使用Flink作为我们的流处理引擎，对于标签的元数据管理我们还是依赖Mysql作为把标签的管理，并使用Airflow作为我们的调度任务框架，并最终将结果输出到Mysql和Hbase中。

## 画像标签体系架构

梳理标签体系是实现用户画像过程中最基础、也是最核心的工作，后续的建模、数据仓库搭建都会依赖于标签体系。
标签是某一种用户特征的符号表示。是一种内容组织方式，是一种关联性很强的关键字，能方便的帮助我们找到合适的内容及内容分类。
标签解决的是描述或命名问题，但在实际应用中，还需要解决数据之间的关联，所以通常将标签作为一个体系来设计，以解决数据之间的关联问题。

用户画像标签体系创建后一般要包含以下几个方面的内容：
1. 画像标签的命名
2. 画像标签的主题
3. 画像标签的属性
4. 画像标签的分类
5. 画像标签的层级
6. 画像标签的依赖


### 画像标签的分类

画像标签按照构造方式、数据类型、业务需求等方面进行分类，每一种分类方式都代表可以采取一种检索标签的方式。

#### 按照打标签的方式

按照打标签的方式来看，一般分为三种类型：
1. 基于统计类的画像标签
2. 基于规则类的画像标签
3. 基于挖掘类的画像标签

统计类标签：这类标签是最为基础也最为常见的标签类型，例如对于某个用户来说，他的性别、年龄、城市、星座、近7日活跃时长、近7日活跃天数、近7日活跃次数等字段可以从用户注册数据、用户访问、消费类数据中统计得出。该类标签构成了用户画像的基础；
规则类标签：该类标签基于用户行为及确定的规则产生。例如对平台上“消费活跃”用户这一口径的定义为近30天交易次数>=2。在实际开发画像的过程中，由于运营人员对业务更为熟悉、而数据人员对数据的结构、分布、特征更为熟悉，因此规则类标签的规则确定由运营人员和数据人员共同协商确定；
机器学习挖掘类标签：该类标签通过数据挖掘产生，应用在对用户的某些属性或某些行为进行预测判断。例如根据一个用户的行为习惯判断该用户是男性还是女性，根据一个用户的消费习惯判断其对某商品的偏好程度。该类标签需要通过算法挖掘产生。

#### 按照标签的主题

按照标签的主题来看，一般分多种类型：
1. 用户(项目)属性画像标签
2. 用户(项目)行为画像标签
3. 用户(项目)偏好画像标签
4. 用户(项目)风险画像标签
......

#### 按照数据的类型(待定)

按照数据的类型来看，一般分为两种类型：
1. 定性画像标签
2. 定量画像标签

定性标签：人为定性的标签，快捷方便但缺少数据验证，可深入数据挖掘
定量标签：根据数据统计或者数据分析得到的标签，数据得到充分验证，但是难以挖掘深层特征。

#### 按照标签的构造方式(待定)

按照标签的构造方式来看，可以分为四种类型：
1. 原始数据层画像标签
2. 事实标签层画像标签
3. 模型标签层画像标签
4. 预测层画像标签

原始数据层：对原始数据，我们主要使用文本挖掘的算法进行分析如常见的TF-IDF、TopicModel主题模型、LDA 、深度学习等算法，主要是对原始数据的预处理和清洗，对用户数据的匹配和标识。
事实标签层：通过文本挖掘的方法，我们从数据中尽可能多的提取事实数据信息，如人口属性信息，用户行为信息，消费信息等。其主要使用的算法是分类和聚类。分类主要用于预测新用户，信息不全的用户的信息，对用户进行预测分类。聚类主要用于分析挖掘出具有相同特征的群体信息，进行受众细分，市场细分。对于文本的特征数据，其主要使用相似度计算，如余弦夹角，欧式距离等。
模型标签层：使用机器学习的方法，结合推荐算法。模型标签层完成对用户的标签建模与用户标识。其主要可以采用的算法有回归，决策树，支持向量机等。通过建模分析，我们可以进一步挖掘出用户的群体特征和个性权重特征，从而完善用户的价值衡量，服务满意度衡量等。
预测层：也是标签体系中的营销模型预测层。这一层级利用预测算法，如机器学习中的监督学习，计量经济学中的回归预测，数学中的线性规划等方法。实习对用户的流失预测，忠实度预测，兴趣程度预测等等，从而实现精准营销，个性化和定制化服务。


### 画像标签的分级

标签需要进行分级分类的管理，一方面使得标签更加的清晰有条件，另一方面也方便我们对标签进行存储查询，也就是管理标签。

把标签分成不同的层级和类别：
1. 方便管理数千个标签，让散乱的标签体系化。
2. 维度并不孤立，标签之间互有关联
3. 可以为标签建模提供标签子集。

同时需要注意梳理某类别的子分类时，尽可能的遵循MECE原则（相互独立、完全穷尽），尤其是一些有关用户分类的，要能覆盖所有用户，但又不交叉。
比如：用户活跃度的划分为核心用户、活跃用户、新用户、老用户、流失用户，用户消费能力分为超强、强、中、弱，这样按照给定的规则每个用户都有分到不同的组里。


## 画像标签体系展示

### 标签主题表

| 标签主题     | 创建人   | 创建时间               |
|----------|-------|--------------------|
| 用户(项目)属性 | QS    | 2022/8/30 15:30:46 |
| 用户(项目)行为 | QS    | 2022/8/30 15:30:46 |
| 用户(项目)偏好 | Eliot | 2022/8/30 15:30:46 |
| 用户(项目)风险 | Eliot | 2022/8/30 15:30:46 |

### 标签类型表

| 标签类型   | 创建人   | 创建时间               |
|--------|-------|--------------------|
| 统计类    | QS    | 2022/8/30 15:30:46 |
| 规则类    | QS    | 2022/8/30 15:30:46 |
| 挖掘类    | Eliot | 2022/8/30 15:30:46 |

### 开发方式表

| 开发方式 | 创建人   | 创建时间               |
|------|-------|--------------------|
| 人为分类 | QS    | 2022/8/30 15:30:46 |
| 机器统计 | Eliot | 2022/8/30 15:30:46 |
| 规则统计 | Eliot | 2022/8/30 15:30:46 |
| 算法预测 | Eliot | 2022/8/30 15:30:46 |

### 开发人表

| 开发人 | 创建人   | 创建时间               |
|-----|-------|--------------------|
| SJ  | QS    | 2022/8/30 15:30:46 |
| LX  | QS    | 2022/8/30 15:30:46 |
| GWD | Eliot | 2022/8/30 15:30:46 |

### 标签桶表

| 标签桶     | 创建人   | 创建时间               |
|---------|-------|--------------------|
| EOA地址标签 | QS    | 2022/8/30 15:30:46 |
| 合约标签    | QS    | 2022/8/30 15:30:46 |
| 项目标签    | Eliot | 2022/8/30 15:30:46 |

### 标签依赖表

对于一些需要其他标签统计、计算和预测的标签，记录该标签依赖的标签

| 标签名              | 依赖标签                          | 创建人 | 创建时间               |
|------------------|-------------------------------|-----|--------------------|
| is_fake_phishing | call_contract_count           | QS  | 2022/8/30 15:30:46 |
| is_fake_phishing | avg_eth_receive_amount        | QS  | 2022/8/30 15:30:46 |
| is_fake_phishing | max_eth_receive_count_per_day | QS  | 2022/8/30 15:30:46 |

### 标签层级表

标签层级应该是树或者森林，所以每个标签只有一个上级标签。
层级主要是概念上的层级。

| 标签名                        | 上级标签             | 级别  | 创建人 | 创建时间               |
|----------------------------|------------------|-----|-----|--------------------|
| max_eth_send_count_per_day | ether_send_count | 2   | QS  | 2022/8/30 15:30:46 |
| avg_eth_send_count_day     | ether_send_count | 2   | QS  | 2022/8/30 15:30:46 |

### 画像标签表

| 标签ID    | 标签名称                       | 主题       | 标签类型 | 数据类型 | 标签桶     | 上级标签             | 描述           | 参数  | 开发方式 | 开发人 | 创建时间               |
|---------|----------------------------|----------|------|------|---------|------------------|--------------|-----|------|-----|--------------------|
| 0000001 | activate_days              | 用户(项目)行为 | 统计类  | Int  | EOA地址标签 |                  | 活跃时间         | {}  | 机器统计 | QS  | 2022/8/30 15:30:46 |
| 0000002 | max_eth_send_count_per_day | 用户(项目)行为 | 统计类  | Int  | EOA地址标签 | ether_send_count | 一天内转出eth最高次数 | {}  | 机器统计 | QS  | 2022/8/30 15:30:46 |
