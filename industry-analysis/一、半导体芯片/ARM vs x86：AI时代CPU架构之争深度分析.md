# ARM vs x86：AI时代CPU架构之争深度分析

> 数据截止日期：2026年7月20日
> 数据来源：Dell'Oro Group、IDC、Canalys、TrendForce、Phoronix、Tom's Hardware、Notebookcheck、Signal65、CSDN技术分析、财联社等

---

## 一、核心问题回答：ARM是否在各方面都优于x86？

**简短回答：不是。ARM并非在所有方面都优于x86，但在AI时代的关键 battlegrounds——能效、多核并发吞吐、AI推理能效比、TCO（总拥有成本）——ARM确实正在建立结构性优势。x86在单核峰值性能、软件兼容性、浮点运算和传统企业级生态方面仍然领先。两条架构的差距正在缩小而非扩大，但AI工作负载的特性确实在系统性地偏向ARM。**

理解这个问题的关键在于：AI工作负载并不像传统PC工作负载那样依赖单核峰值性能或x86软件兼容性。AI推理和智能体调度更依赖高并发吞吐、内存带宽和能效比——这恰好是ARM架构的基因优势。随着AI从训练走向推理、从数据中心走向边缘和终端，ARM的市场份额扩张具有结构性必然性。

---

## 二、架构本质差异：CISC vs RISC的深层影响

### 2.1 指令集设计哲学

x86采用CISC（复杂指令集），指令长度可变（1-15字节），基础指令超过1,000条，单条指令可完成多步操作。设计目标是"硬件做更多，软件做更少"——将复杂度从编译器转移到硬件解码器。优势是单条指令信息密度高，适合计算密集型任务；代价是硬件解码逻辑复杂，功耗高。

ARM采用RISC（精简指令集），所有指令固定32位宽度，基础指令约400条，严格Load/Store架构（只有ldr/str指令能访问内存，运算只操作寄存器）。设计目标是"硬件极简，编译器优化"——降低硬件复杂度以换取低功耗和高频率。优势是解码器简单、功耗低、晶体管利用率高；代价是复杂操作需要更多指令完成。

### 2.2 架构差异对AI工作负载的影响

AI工作负载有几个关键特征：大规模矩阵运算（由GPU/NPU承担）、高并发任务调度（CPU核心数量决定）、海量内存带宽需求（数据搬运是瓶颈）、长时间持续运行（能效比决定运营成本）。这些特征对CPU架构的要求与传统PC完全不同：

在多核并发能力上，ARM天然适合高并发。同等功耗下ARM可部署的核心数量远超x86——AWS Graviton4在同等功耗下提供96核，而Intel Xeon最高56核、AMD EPYC最高128核但功耗也更高。AI推理任务（尤其是智能体调度、RAG检索、工具调用）天然可并行化，核心数量比单核性能更重要。

在内存带宽上，ARM的Load/Store架构配合宽位内存接口（Graviton4的12通道DDR5-5600）能提供更高内存带宽。NVIDIA Grace CPU采用LPDDR5X统一内存，带宽达1.2TB/s，是传统x86服务器CPU的3倍。AI推理的瓶颈往往不在算力而在数据搬运，内存带宽优势直接转化为推理性能优势。

在能效比上，ARM的简化解码流水线、精细时钟门控和DVFS（动态电压频率调整）使每瓦性能显著高于x86。实测数据显示，ARM集群每机架功耗比x86低33%，每瓦SPECint性能高52%，年电费节省33%。对于需要7×24小时运行的AI推理服务，能效差异直接转化为巨大的运营成本差异。

### 2.3 x86仍然领先的领域

单核峰值性能方面，x86仍然是行业标杆。在浮点运算、3D渲染、大型数据库OLTP事务、CAD工业建模等计算密集型场景，Intel酷睿i9单核性能可达ARM旗舰的1.6倍以上。SPECint_rate基准测试中，Intel Xeon得分350，ARM Ampere Altra得分420（ARM领先20%），但在SPECfp（浮点）测试中x86仍占优势。

软件兼容性是x86最大的护城河。近50年沉淀的软件生态使几乎所有闭源商业软件、工业控制软件、老旧企业系统都仅提供x86二进制包。虽然ARM可通过模拟器（Rosetta 2、Prism）运行x86程序，但存在10-30%的性能损耗，且内核级驱动（反作弊软件、VPN驱动、工业控制驱动）完全无法模拟。

虚拟化和企业级生态成熟度上，VMware、Hyper-V、KVM对x86深度优化了二十余年，传统ERP、金融核心系统全部基于x86开发。ARM在这些领域的生态完善仍需3-5年。

---

## 三、市场份额变化：ARM的进攻路线图

### 3.1 服务器/数据中心市场：ARM从7%到25%的飞跃

这是ARM攻势最猛的战场。根据Dell'Oro Group数据，ARM CPU在服务器市场的份额变化如下：

| 时间 | ARM服务器CPU份额 | 主要驱动力 |
|------|-----------------|-----------|
| 2022 Q2 | 7.1% | AWS Graviton独力支撑 |
| 2024 Q2 | 15% | Graviton3/4 + Google Axion + Microsoft Cobalt |
| 2025 Q2 | **25%** | NVIDIA Grace/Blackwell (GB200/GB300) 大规模部署 |
| 2025全年（IDC预估） | 21.1%（出货量） | ARM服务器出货量同比激增70% |
| 2028年（预测） | 25-28% | NVIDIA Vera + Qualcomm/Fujitsu ARM服务器CPU |

ARM的50%目标（Arm基础设施主管Mohamed Awad设定）虽然未能实现，但一年内从15%跃升至25%的增速仍然惊人。核心增量变量是NVIDIA——Grace CPU的营收规模已经能与云厂商自研ARM CPU相提并论。每个GB300 NVL72机架配备72个Blackwell GPU和36个Grace CPU，随着NVIDIA AI服务器出货量爆发，ARM CPU数量自然水涨船高。

**x86服务器市场仍占79%份额（2025年），规模约$2,839亿。**但x86的增长主要来自AI服务器中搭配的Intel Xeon/AMD EPYC，而ARM的增长除了云厂商自研芯片外，还吃掉了大量传统Web服务、微服务、容器化工作负载。

### 3.2 PC/客户端市场：ARM从15%到30%的渐进渗透

PC市场的ARM化进程比服务器慢得多，主要由苹果和高通推动：

| 时间 | ARM在PC市场份额 | 主要驱动力 |
|------|----------------|-----------|
| 2022 Q3 | ~13.1% | Apple M系列 + Chromebook |
| 2025年 | ~13-15% | Apple M3/M4 + Snapdragon X Elite第一代 |
| 2026年（Canalys预测） | **~30%** | Apple M4/M5 + Snapdragon X2 Elite |
| 2029年（TrendForce预测） | ~11.5%（仅WoA笔记本） | 多厂商入局（NVIDIA/MediaTek等） |

需要注意的是，Canalys的30%预测包含了苹果Mac（100% ARM）和Chromebook中的ARM份额。如果只看Windows on ARM（WoA）笔记本，TrendForce 2026年初预测渗透率仅3.2%，2029年才达到11.5%。但这一预测可能偏保守——Snapdragon X2 Elite的评测表现远超预期，且NVIDIA+MediaTek将在2026下半年入局WoA市场。

### 3.3 移动设备市场：ARM的绝对领地

在智能手机和平板电脑市场，ARM占据99%以上份额，几乎没有x86的存在。这个市场早已不是争夺焦点。值得注意的是，移动端的AI推理（手机端LLM、AI摄影、语音助手）全部运行在ARM NPU上，这意味着AI开发者工具链对ARM的优先支持正在形成正循环。

---

## 四、主要ARM CPU产品力分析

### 4.1 服务器/数据中心ARM CPU

**AWS Graviton4：**

基于Arm Neoverse-V2内核（Armv9.0 ISA），96核，每核2MB L2缓存，12通道DDR5-5600内存。AWS宣传比Graviton3性能提升30%，Web应用性能提升30%、数据库性能提升40%、Java性能提升40%+。Signal65咨询公司的分析显示，Graviton4不仅在价格性能比上领先，在整体性能上也显著超越AMD EPYC和Intel Xeon。AWS称Graviton实例比可比x86实例提供"高达40%的更好价格性能比"。

**Microsoft Azure Cobalt 200：**

2025年11月发布，基于Arm Neoverse CSS V3平台，支持Armv9架构特性（机密计算架构CCA和第二代SVE2）。微软已在Build 2026上宣布早期预览，2026年大规模部署。Cobalt 200专门为云原生工作负载优化，针对Azure SQL和容器密度进行了深度调优。

**Google Axion：**

Google Cloud自研ARM服务器CPU，基于Neoverse V2核心。已在大规模生产环境部署，主要服务Google内部工作负载和GCP客户实例。

**NVIDIA Grace CPU（当前一代）：**

72核Neoverse V2核心，通过NVLink-C2C接口与Blackwell GPU紧密耦合。在GB200/GB300 NVL72机架中，36个Grace CPU + 72个Blackwell GPU组成统一计算平面，无需x86 CPU参与。Grace CPU的营收已与云厂商自研ARM CPU相提并论——这意味着NVIDIA不仅是GPU供应商，正在成为ARM服务器CPU的重要玩家。

**NVIDIA Vera CPU（下一代）：**

2026年3月GTC发布，2026年5月开始向首批客户交付。88核自研Olympus核心（Armv9.2-A），176线程，单芯片最高1.5TB LPDDR5X内存，带宽1.2TB/s。NVIDIA宣称：每sandbox性能是x86的1.5倍、每核内存带宽是x86的3倍、能效是x86的2倍、单线程性能比Grace提升50%、sandbox处理速度提升70%。Vera定位为"全球首款专为Agentic AI打造的服务器CPU"——解决AI智能体的海量沙箱、工具调用、长上下文调度需求。单颗售价超$20,000，机构预测全年营收有望突破$200亿。

**AmpereOne（Ampere Computing）：**

专注云原生工作负载的独立ARM服务器CPU厂商。2025年被软银（Arm控股方）以$65亿收购，软银正在整合ARM IP + Ampere设计能力 + Alphawave SerDes技术，构建从IP到芯片的完整ARM服务器生态。

### 4.2 PC/客户端ARM CPU

**Apple M4/M5系列：**

苹果M4 Ultra（2025年5月发布）：32核4.52GHz，ARMv9架构，80核GPU。在Geekbench 6多核测试中与AMD Ryzen AI Max+ 395（16核3.0GHz x86）竞争激烈。Apple Silicon的最大优势是垂直整合——SoC、操作系统、应用层全由苹果控制，不存在兼容性问题。MacBook续航18-22小时是x86笔记本难以匹敌的。

**Qualcomm Snapdragon X2 Elite：**

2026年4月上市，第二代Oryon CPU架构。三个SKU，旗舰Extreme版18核（12 Prime + 6 Performance），最高5.0GHz，集成LPDDR5X 192-bit，带宽228 GB/s，80 TOPS NPU。

评测结果令人震惊：Geekbench 6单核3,807分，Cinebench 2026多核超6,000分，比Intel最新Panther Lake旗舰快24%。关键优势是"电池上近满性能"——Snapdragon笔记本在电池模式下保持接近满血性能，而Intel/AMD笔记本拔电后性能损失约50%。第三代Oryon架构单核性能提升35%，功耗降低43%。

但兼容性仍是软肋。Prism翻译器对普通应用表现不错，但内核级驱动（反作弊软件、VPN、工业控制）无法模拟，企业采购仍高度保守。

**NVIDIA + MediaTek "Spark"系列（即将入局）：**

高通在Windows on ARM的独占协议到期后，NVIDIA联手MediaTek开发ARM PC处理器，代号"Spark"。预计集成RTX GPU架构和强大本地AI算力，2026下半年问世。这将打破高通独占格局，为WoA市场注入强劲竞争。

### 4.3 服务器端性能实测对比

基于Phoronix在AWS EC2上的同等配置实测（16 vCPU + 64GB RAM，M8实例）：

| 测试项目 | Intel Xeon (m8i) | AMD EPYC (m8a) | AWS Graviton4 (m8g) | 最优 |
|---------|-----------------|----------------|---------------------|------|
| SPECint_rate2017 | 350 | — | 420 | Graviton4 (+20%) |
| SPECfp_rate2017 | 280 | — | 310 | Graviton4 (+11%) |
| Redis QPS | 1,200,000 | — | 950,000 | Xeon (+26%) |
| Nginx连接处理 | 85,000 | — | 92,000 | Graviton4 (+8%) |
| TensorFlow推理 | 450 img/s | — | 520 img/s | Graviton4 (+16%) |
| miniFE (HPC) | 基准 | Genoa可比 | 显著领先Genoa | Graviton4 |
| 加密基准 | 基准 | 可比 | 大幅领先前代 | Graviton4 |

在Web服务、AI推理、高并发场景，Graviton4已系统性超越x86。但在Redis内存数据库（依赖单线程低延迟）等场景x86仍有优势。

---

## 五、AI如何改变CPU架构竞争格局

### 5.1 AI工作负载的特殊性

传统PC/服务器工作负载的特征是：单用户、低并发、依赖单核性能、依赖x86软件生态。AI时代的工作负载特征完全不同：

AI训练阶段：GPU/NPU承担绝大部分计算，CPU主要负责数据预处理和任务调度。CPU性能不是瓶颈，但CPU功耗会影响整体能效。Grace CPU/Vera CPU之所以用ARM架构，正是因为在GPU为主的AI训练机架中，CPU需要"足够好但足够省电"——ARM完美匹配这一需求。

AI推理阶段：比训练更依赖CPU。推理需要处理大量并发请求、管理KV缓存、调度工具调用。NVIDIA Vera CPU正是瞄准这一场景——88核高并发、1.2TB/s内存带宽、针对sandbox处理优化70%。x86 CPU在推理场景的能效比明显不如ARM。

AI智能体（Agentic AI）阶段：这是最革命性的变化。AI智能体需要创建大量沙箱环境、执行工具调用、维护长上下文、进行强化学习。传统x86 CPU无法匹配智能体的高并发、高内存带宽需求，导致高端GPU算力闲置、集群利用率不足50%。Vera CPU的出现标志着算力产业进入"CPU+GPU双核心协同"时代。

### 5.2 NPU集成与端侧AI

在端侧AI（AI PC、AI手机）领域，NPU算力已成为CPU的标配指标。微软Copilot+ PC标准要求NPU算力≥40 TOPS。当前各阵营NPU能力：

- Snapdragon X2 Elite：80 TOPS NPU
- Apple M4系列：38 TOPS Neural Engine
- Intel Panther Lake (Core Ultra Series 3)：~48 TOPS NPU
- AMD Ryzen AI 400 (Gorgon Point)：~50 TOPS NPU

ARM阵营在NPU集成上并不占绝对优势——Intel和AMD都在快速提升NPU能力。但ARM的SoC集成设计（CPU+GPU+NPU统一封装）在功耗效率上优于x86的传统分立设计。

### 5.3 AI对x86可寻址市场的侵蚀

AI正在从两个方向侵蚀x86的市场基础：

在数据中心，AI服务器越来越采用"GPU+ARM CPU"架构（如GB300 NVL72），而非传统的"GPU+x86 CPU"架构。每个AI服务器机架中的x86 CPU数量正在减少——GB300机架完全不需要x86 CPU。随着AI服务器在数据中心占比从当前的~50%进一步提升，x86 CPU的服务器出货量将受到结构性压缩。

在客户端，AI PC的NPU算力需求推动SoC级别集成，而ARM的SoC设计经验（来自手机端十几年积累）远超x86阵营。当PC从"CPU为主"转向"SoC为主"时，ARM的设计哲学更有优势。

---

## 六、x86阵营的应对策略

### 6.1 Intel的反击

**产品路线图：**

- Panther Lake（Core Ultra Series 3，2026 Q2）：采用Intel 18A工艺，全新CPU和GPU架构（Xe2集成显卡），最高16混合核心，大幅提升能效。这是Intel首次在笔记本上将电池续航拉升至15-18小时，直接回应ARM的能效优势。
- Nova Lake（Core Ultra Series 4，2027年）：桌面版顶配52核（16 P核 + 32 E核 + 4 LP核），Intel史上最激进的规格。桌面版跳过Panther Lake直接从Arrow Lake升级。
- Diamond Rapids（服务器CPU，2026-2027年）：接替Granite Rapids，专注AI推理CPU需求。但大摩指出Diamond Rapids落后于AMD Venice（Zen 6 EPYC），服务器产品力仍存疑。
- Clearwater Forest（2027-2028年）：下一代服务器CPU，14A工艺。

**战略调整：** Intel将芯片产能从消费级向数据中心CPU和AI芯片倾斜。18A工艺良率提升至85%，EMIB封装良率达98%——这些技术也用于提升自身芯片的竞争力。CEO陈立武（Lip-Bu Tan）强调"AI推理CPU"作为DCAI的核心增长引擎，Q1 2026该分部营收+22%。

**能效反击：** Lunar Lake和Panther Lake标志着Intel在能效上的根本性转变。2026年最新x86笔记本日常续航已达15-18小时，极大削弱了ARM的能效壁垒。Let's Talk Gizmo的评测指出："2026年的故事不是ARM轻松赢得能效桂冠，而是Intel正在缩小差距，而Qualcomm为了追求原始性能牺牲了部分能效优势。"

### 6.2 AMD的反击

**产品路线图：**

- Zen 5架构（当前）：Ryzen AI 300系列（Strix Point）和EPYC Turin已在市场。Strix Point的NPU算力约50 TOPS。
- Medusa Point（Ryzen AI 400，2026 Q2）：Zen 6架构，台积电2nm工艺，旗舰加速频率突破6GHz，首次引入12核CCD设计（取代8核CCD），延续AM5插槽兼容性。
- EPYC Venice（Zen 6服务器CPU，2026年）：大摩认为Venice在服务器产品力上超越Intel Diamond Rapids。
- Zen 6桌面Ryzen：推迟至2027年CES发布，与Intel Nova Lake同台竞争。

**3D V-Cache优势：** AMD延续并升级3D V-Cache技术，在游戏性能和缓存敏感型AI推理任务上保持竞争力。大缓存设计是对ARM多核优势的一种x86式回应——用更大缓存弥补核心数量劣势。

**服务器份额收割：** AMD预计2026年底服务器收入份额达到50%或超越Intel。在ARM侵蚀x86整体市场的同时，AMD正在从Intel手中夺取x86内部份额——这是"在沉没的船上抢头等舱"的策略。

### 6.3 x86联盟的生态防御

Intel和AMD在2024年成立了x86生态系统顾问小组（x86 Ecosystem Advisory Group），旨在简化x86架构碎片化、统一指令集扩展。这是x86双雄首次在架构层面正式合作应对ARM威胁——历史上两家公司在x86指令集实现上各有分叉（如AVX-512支持差异），统一生态有助于降低软件开发成本。

但x86的根本困境在于商业模式：Intel和AMD都是自研自产CPU，封闭芯片供应，不授权IP。ARM的授权模式允许任何厂商定制SoC——苹果、高通、AWS、Google、Microsoft、NVIDIA都在设计自己的ARM芯片。这种"众人拾柴"的生态扩展速度远超x86两家公司的内部研发。

---

## 七、竞合关系：敌人还是朋友？

AI时代的CPU竞争不是简单的零和博弈。几家关键公司之间存在复杂的竞合关系：

**NVIDIA与Intel/AMD：** NVIDIA的Grace/Vera CPU采用ARM架构，直接替代了AI服务器中的x86 CPU——这是竞争关系。但同时，NVIDIA的GeForce GPU需要安装在x86 PC主板上，NVIDIA也是Intel代工18A/14A的客户——这是合作关系。NVIDIA的立场是"在AI数据中心用ARM替代x86，在游戏PC上继续依赖x86"。

**Apple与Qualcomm：** 两者都在ARM PC市场，但苹果控制全栈（芯片+OS+应用），高通只做芯片依赖微软Windows。苹果的垂直整合使其不受兼容性困扰，而高通必须面对WoA生态的不成熟。两者是平行竞争关系。

**AWS/Google/Microsoft与Intel/AMD：** 三大云厂商自研ARM CPU直接减少了对Intel Xeon/AMD EPYC的采购——这是替代关系。但三大云厂商仍然大量采购x86服务器（占其服务器总数的75%+），且x86实例仍是利润最高的云产品。短期内是"ARM增量、x86存量"的共存格局。

**ARM公司与Intel/AMD：** ARM公司本身不造芯片，只卖IP授权。Intel和AMD都不是ARM的授权客户——他们自有x86指令集。但ARM的授权客户（苹果、高通、AWS、NVIDIA等）正在系统性地侵蚀Intel/AMD的市场。ARM公司2025年服务器CPU授权收入预计增量$4-6亿。

---

## 八、综合判断与未来展望

### 8.1 核心判断

ARM并非在所有方面都优于x86。更准确的表述是：**AI时代的工作负载特性正在系统性地偏向ARM的架构优势区域。**

在能效比上，ARM结构性领先。每瓦性能高52%，每机架功耗低33%。这对7×24小时运行的AI推理服务至关重要。x86通过Lunar Lake/Panther Lake在笔记本端缩小了差距，但服务器端的能效差距仍然显著。

在多核并发上，ARM天然适合。96核Graviton4 vs 56核Xeon，ARM在Web服务、容器化、微服务、AI推理等高并发场景系统性胜出。x86通过AMD 128核EPYC回应，但功耗代价更大。

在AI推理性能上，ARM已建立优势。Graviton4的TensorFlow推理比Xeon快16%；NVIDIA Vera CPU号称每sandbox性能是x86的1.5倍。AI推理的瓶颈是内存带宽和并发调度，这恰好是ARM的强项。

在单核峰值性能上，x86仍领先。浮点运算、3D渲染、大型数据库OLTP事务——这些场景x86的复杂指令集优势明显。但这些场景在AI时代的重要性正在下降。

在软件兼容性上，x86的护城河仍然很深。但需要区分两个市场：在传统企业IT市场（Oracle DB、SAP、VMware），x86的兼容性优势几乎无法撼动；在云原生和AI工作负载市场（容器化微服务、Python/Go/Rust应用、AI推理服务），软件已天然跨架构，x86的兼容性优势不存在。

在TCO上，ARM结构性优势。100节点集群三年TCO：x86 $762.8万 vs ARM $603.2万，节省$159.6万（21%）。对超大规模数据中心，这个差异放大到数亿美元。

### 8.2 预测

服务器市场：ARM份额从当前25%继续上升，2028年达到25-30%。NVIDIA Vera CPU是最大增量变量——如果Vera如宣传般性能出色，AI服务器中的x86 CPU将被加速替代。但传统企业IT服务器（运行Oracle/SAP/VMware）仍将是x86堡垒，ARM难以渗透。总体而言，x86在服务器市场将从79%逐步降至65-70%。

PC市场：ARM份额从15%向25-30%渐进渗透。苹果Mac保持100% ARM，Snapdragon X2 Elite推动WoA笔记本渗透率从3.2%提升，NVIDIA+MediaTek入局加速生态成熟。但游戏PC、工作站、企业采购市场仍将以x86为主。Windows on ARM的兼容性壁垒需要3-5年才能真正突破。

移动市场：ARM继续保持99%+垄断，无悬念。

AI芯片市场：ARM在AI推理CPU领域将建立统治地位。NVIDIA Vera CPU + AWS Graviton + Microsoft Cobalt + Google Axion形成全方位ARM推理CPU矩阵。x86 CPU在AI训练机架中的角色将逐渐边缘化——当GPU可以直连ARM CPU并通过NVLink通信时，x86 CPU在AI服务器中的存在理由（管理I/O、运行OS）完全可以用更省电的ARM替代。

### 8.3 对投资者的启示

对Intel而言，ARM的进攻同时发生在其两大核心市场（服务器+客户端），但Intel的18A代工战略恰好可以将ARM的威胁转化为代工收入——NVIDIA、AMD、OpenAI都已成为18A客户。Intel的命运取决于能否在"失去x86份额"和"获得代工收入"之间实现正净切换。

对AMD而言，ARM在服务器端的扩张是双重打击——既侵蚀x86总量，又可能在AMD达到50%服务器份额之前改变游戏规则。但AMD的Zen 6 + 3D V-Cache在AI推理性能上仍有竞争力，且AMD也是Intel 18A代工客户，有潜在的ARM芯片设计能力。

对NVIDIA而言，Vera CPU开辟了GPU之外的第二增长曲线，机构预测全年营收突破$200亿。NVIDIA从"GPU公司"变为"GPU+CPU公司"，且两端都用ARM架构——这意味着NVIDIA对x86生态的依赖正在下降。

对ARM Holdings而言，服务器CPU授权收入是未来5年最大的增长引擎。当前ARM年出货约400-600万颗服务器CPU，占全球14%；预计2028年达到25-28%，对应的年营收增量约$4-6亿。加上PC端Snapdragon X2和NVIDIA Spark的授权收入，ARM公司的IP授权模式在AI时代具有独特的杠杆效应。

---

> 数据来源声明：本文市场份额数据来自 Dell'Oro Group、IDC、Canalys、TrendForce、Mercury Research；性能基准数据来自 Phoronix、Signal65、Tom's Hardware、Notebookcheck；产品规格和路线图数据来自各厂商官方发布及 tech-news 媒体报道（CSDN、财联社、网易科技、Tom's Hardware、AnandTech 等）。部分预测数据已标注来源和预测年份。

