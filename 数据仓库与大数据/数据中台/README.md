# 数据中台

阿里在 2018 年提出了所谓“数据中台”的概念：即数据被统一采集，规范数据语义和业务口径形成企业基础数据模型，提供统一的分析查询和新业务的数据对接能力。数据中台并不是新的颠覆式技术，而是一种企业数据资产管理和应用方法学，涵盖了数据集成、数据质量管理、元数据与主数据管理、数仓建模、支持高并发访问的数据服务接口层开发等内容。

在数据中台建设中，结合企业自身的业务需求特点，架构和功能可能各不相同，但其中一个最基本的需求是数据采集的实时性和完整性。数据从源端产生，到被采集到数据汇集层的时间要尽可能短，至少应做到秒级延迟，这样中台的数据模型更新才可能做到近实时，构建在中台之上依赖实时数据流驱动的应用（例如商品推荐、欺诈检测等）才能够满足业务的需求。

以阿里双十一为例，在极高的并发情况下，订单产生到大屏统计数据更新延迟不能超过 5s，一般在 2s 内。中台对外提供的数据应该是完整的，源端数据的 Create、Update 和 Delete 都要能够被捕获，不能少也不能多，即数据需要有端到端一致性的能力（Exactly Once Semantic，EOS）。当然，EOS 并非在任何业务场景下都需要，但从平台角度必须具备这种能力，并且允许用户根据业务需求灵活开启和关闭。

# 数据中台的产生背景

起初，企业只有一个主营业务，比如电商，但随着公司战略和发展需要，会新增多支业务线，由于存在负责业务线开发的团队不一致，随之而来的就是风格迥异的代码风格和数据烟囱问题。

数据中台的产生就是为了解决数据烟囱的问题，打通数据孤岛，让数据活起来，让数据产生价值，结合前台能力，达到快速响应用户的目标。

中台只会同步能服务于超过两个业务线的数据，如果仅仅带有自身业务属性(不存在共性)的数据，不在中台的考虑范围内。例如:电商的产品产地信息，对于金融业务来说，其实是没有价值的，但电商的用户收货地址对金融业务来说是有价值的。所以不要简单的认为数据中台会汇集企业的所有数据，还是有侧重点的。导致这个结果的原因还包括数据中台建设本身是一个长周期的事，如果数据仅仅作用于一方，由业务方(前台)自行开发，更符合敏捷开发的特性。

关于何时应该建立数据中台这个问题，我的思考是这样的。复杂的业务线、丰富的数据维度和公司上层领导主推。三者缺一，都没有实行的必要。
一只手都能数的过来的业务线量，跨多个项目的需求相对还是比较少的，取数也比较方便，直接走接口方式基本就能满足。反而，通过数据中台流转，将问题复杂化了。

数据的维度越丰富，数据的价值越大。只知道性别数据，与知道性别和年龄，所得到的用户画像，肯定是维度丰富的准确性高。维度不丰富的情况下，没有计算的价值。

可能会很奇怪为什么一定需要公司上层的同意。这里就可能涉及到动了谁的奶酪的问题，数据是每个业务线最重要的资源，在推行中台过程中，势必会遇到阻力，只有成为全公司的战略任务，才有可能把事情做好。

中台如果没有考虑通用的业务能力，也会导致无法更专注于对中台技术的深入研究。中台如果不从抽象度、共性等角度出发，很有可能局限于某单一业务，导致中台无法很好地适应其他相关业务的要求，从而不能很好地应对业务的变化。如果中台的抽象程度低、扩展性差，则会导致中台无法满足前台业务需求。这时前台应用又因为业务本身的发展目标和压力不得不自行组织团队完成这部分功能，由此可能发生本应由中台提供的能力却最终实现在业务应用中，失去了中台存在的价值。

![中台架构图](https://s2.ax1x.com/2019/10/20/Ku7Faj.jpg)