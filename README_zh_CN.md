# Hyperledger FireFly

[![codecov](https://codecov.io/gh/hyperledger/firefly/branch/main/graph/badge.svg?token=QdEnpMqB1G)](https://codecov.io/gh/hyperledger/firefly)
[![Go Report Card](https://goreportcard.com/badge/github.com/hyperledger/firefly)](https://goreportcard.com/report/github.com/hyperledger/firefly)
[![FireFy Documentation](https://img.shields.io/static/v1?label=FireFly&message=documentation&color=informational)](https://hyperledger.github.io/firefly//)
![build](https://github.com/hyperledger/firefly/actions/workflows/docker_main.yml/badge.svg?branch=main)
[![OpenSSF Best Practices](https://www.bestpractices.dev/projects/7826/badge)](https://www.bestpractices.dev/projects/7826)

![Hyperledger FireFly](./images/hyperledger_firefly_logo.png)

Hyperledger FireFly 是HyperLedger出品的首款开源超级节点：企业构建和拓展一个安全的Web3应用的全栈式解决方案。

FireFly 的数字资产、数据流和区块链交易上的API能够让企业能够在众多流行区块链和各种流行协议迅速构建起开箱即用的应用。

## 开始使用 Hyperledger FireFly

学习如何去使用FireFly的最佳方案: [文档](https://hyperledger.github.io/firefly).

在这里你能够找到 [开始指引](https://hyperledger.github.io/firefly/latest/gettingstarted/),
在文档的指导下，你几分钟就可以在你的本地机器上运行起来FireFly超级节点。

你的本地机器将会安装上:

FireFly CLI                   |  FireFly Explorer UI                | FireFly Sandbox  |
:----------------------------:|:-----------------------------------:|:----------------:|
[![CLI](./images/firefly_cli.png)](https://hyperledger.github.io/firefly/latest/gettingstarted/firefly_cli/#install-the-firefly-cli) | [![UI](./images/firefly_explorer.png)](https://github.com/hyperledger/firefly-ui) | [![Sandbox](./images/firefly_sandbox.png)](https://hyperledger.github.io/firefly/latest/gettingstarted/sandbox/#use-the-sandbox) |

## 与FireFly社区取得联系

- [加入我们的 Discord](https://discord.gg/hyperledger)

## 技术架构

Hyperledger FireFly 有着插件式的微服务架构。FireFly的所有服务都是插件式的，例如区块链技术、token ERC标准、自定义智能合约甚至到事件分发层以及私有数据库都是可插拔的。

假如你在FireFly的技术架构图中还未发现你喜爱的区块链技术，你也不必担心。FireFly提供了清晰明确的方式可以让你去拓展，从而避免你花费大量时间去重新构建你自己的区块链应用程序。

[![Hyperledger FireFly 技术架构图](./doc-site/docs/images/firefly_architecture_overview.jpg)](https://raw.githubusercontent.com/kaleido-io/firefly/main/doc-site/docs/images/firefly_architecture_overview.jpg)

## 开始向 Hyperledger FireFy 产生贡献

你有很多种方式可以对 Hyperledger FireFly 项目产生贡献，无论你的技术栈是前端、后端或者全栈。

点击 [贡献指引](https://hyperledger.github.io/firefly/latest/contributors/)， **欢迎!**.

## 其他项目库

你当前处在“核心”库中，这是一个用Go语言编写的和有着托管API服务器和中央协调引擎的库。在这里你可找到用各种语言（TypeScript、Java以及其他重量级运行环境组件）编写的插件式微服务组件。

你可能感兴趣的其他项目包含着微服务组件、用户体验、控制台和样例的库如下

> 提示：只罗列开源库和插件

### 区块链连接

- Transaction Manager - https://github.com/hyperledger/firefly-transaction-manager
- RLP & ABI encoding, KeystoreV3 utilities and secp256k1 signer runtime -  https://github.com/hyperledger/firefly-signer
- FFCAPI reference connector for EVM Chains - https://github.com/hyperledger/firefly-evmconnect
  - Public EVM compatible chains: Learn more in the [documentation](https://hyperledger.github.io/firefly)
- Permissioned Ethereum connector - https://github.com/hyperledger/firefly-ethconnect
  - Private/permissioned: Hyperledger Besu / Quorum
- Hyperledger Fabric connector - https://github.com/hyperledger/firefly-fabconnect
- Tezos connector - https://github.com/hyperledger/firefly-tezosconnect
- Corda connector starter: https://github.com/hyperledger/firefly-cordaconnect
  - CorDapp specific customization is required

### Token 标准

- Tokens ERC20/ERC721 - https://github.com/hyperledger/firefly-tokens-erc20-erc721
- Tokens ERC1155 - https://github.com/hyperledger/firefly-tokens-erc1155

### 私有数据总线连接

- HTTPS Data Exchange - https://github.com/hyperledger/firefly-dataexchange-https

### 开发者生态系统

- Command Line Interface (CLI) - https://github.com/hyperledger/firefly-cli
- Explorer UI - https://github.com/hyperledger/firefly-ui
- Node.js SDK - https://github.com/hyperledger/firefly-sdk-nodejs
- Sandbox / Exerciser - https://github.com/hyperledger/firefly-sandbox
- Samples - https://github.com/hyperledger/firefly-samples
- FireFly Performance CLI: https://github.com/hyperledger/firefly-perf-cli
- Helm Charts for Deploying to Kubernetes: https://github.com/hyperledger/firefly-helm-charts

## FireFly 核心代码层级结构

```
┌──────────┐  ┌───────────────┐
│ cmd      ├──┤ firefly   [Ff]│  - 控制台接入
└──────────┘  │               │  - 父级上下文创建
              │               │  - 信号控制
              └─────┬─────────┘
                    │
┌──────────┐  ┌─────┴─────────┐  - HTTP 监听 (Gorilla mux)
│ internal ├──┤ api       [As]│    * TLS (SSL), CORS 配置等.
└──────────┘  │ server        │    * WS 升级在同一端口
              │               │  - REST 路由定义
              └─────┬─────────┘    * 仅限简单的路由逻辑，所有处理均推迟到协调器
                    │
              ┌─────┴─────────┐  - REST 路由定义框架
              │ openapi   [Oa]│    * 标准化 Body，Path，Query， Filter 语义
              │ spec          |      - OpenAPI 3.0 (Swagger) 生成器
              └─────┬─────────┘    * 包括 Swagger. UI
                    │
              ┌─────┴─────────┐  - WebSocket 服务器
              │           [Ws]│    * 开发者友好型的基于JSON的业务应用程序开发
              │ websockets    │    * 可靠的信息序列传递
              └─────┬─────────┘    * _Event interface [Ei] 支持注入其他计算框架/传输_
                    │
              ┌─────┴─────────┐  - 监听数据库事件改变的拓展接口
              │ admin     [Ae]│    * 对于搭建核心服务外的微服务拓展应用可用
              │ events        |    * 可用于事务管理组件
              └─────┬─────────┘    * 特定对象类型过滤
                    │
              ┌─────┴─────────┐  - 核心数据类型
              │ fftypes   [Ft]│    * 用于 API 和序列化
              │               │    * 通过路由定义，APIs 可以实现隐藏输入属性
              └─────┬─────────┘
                    │
              ┌─────┴─────────┐  - 核心运行服务器，初始化和拥有自己的实例:
              │           [Or]│    * Components：特性实现
  ┌───────┬───┤ orchestrator  │    * Plugins：可插拔设施服务
  │       │   │               │  - 向路由暴露行为
  │       │   └───────────────┘    * 所有的API调用将会在这里开始处理
  │       │
  │  Components: 引擎里的组件将会做重量级的任务
  │       │
  │       │   ┌───────────────┐  - 通过跨链技术整合区块链智能合约逻辑
  │       ├───┤ contract  [Cm]│    * 为智能合约生成 OpenAPI 3 / Swagger 定义，并且传播到网络中
  │       │   │ manager       │    * 为了本地区块链事件管理监听, 并且路由这些应用事件
  │       │   └───────────────┘    * 转化 to/from 本地区块链接口 (ABI etc.) 和 FireFly Interface [FFI] 格式
  │       │
  │       │   ┌───────────────┐  - 包含整个网络的视图
  │       ├───┤ network   [Nm]│    * 集成网络的 permissioning [NP] 插件
  │       │   │ map           │    * 集成广播插件
  │       │   └───────────────┘    *控制成员身份、节点身份、签名身份的层级
  │       │
  │       │   ┌───────────────┐  - 广播数据给所有网络中的成员
  │       ├───┤ broadcast [Bm]│    * 实现批量组件的分发
  │       │   │ manager       |    * 集成 共享存储接口 [Ss] 插件
  │       │   └───────────────┘    * 集成区块链接口 [Bi] 插件
  │       │
  │       │   ┌───────────────┐  - 发送私有数据给网络中的成员
  │       ├───┤ private   [Pm]│    * 实现批量组件分发
  │       │   │ messaging     |    * 集成数据交换 [Dx] 插件
  │       │   └──────┬────────┘    * 消息可以通过区块链固定或者排序，或者仅发送
  │       │          │
  │       │   ┌──────┴────────┐  - 有着数据隔离的区块链或者成员群组
  │       │   │ group     [Gm]│    * 集成数据交换 [Dx] 插件
  │       │   │ manager       │    * 集成区块链接口 [Bi] 插件
  │       │   └───────────────┘
  │       │
  │       │   ┌───────────────┐  - 私有数据管理和验证
  │       ├───┤ data      [Dm]│    * 实现批量组件的分发
  │       │   │ manager       │    * 集成数据交换 [Dx] 插件
  │       │   └──────┬────────┘    * 集成区块链接口[Bi] 插件
  │       │          │
  │       │   ┌──────┴────────┐  - JSON数据语法管理和验证 (架构可以拓展到XML或者其他)
  │       │   │ json      [Jv]│    * 站内和站外消息的JSON语法管理和验证
  │       │   │ validator     │    * 语法传播
  │       │   └──────┬────────┘    * 集成广播插件
  │       │          │
  │       │   ┌──────┴────────┐  - 可通过ID或者hash寻址的二进制数据存储
  │       │   │ blobstore [Bs]│    * 集成数据交换 [Dx] 插件
  │       │   │               │    * 对数据进行hash化处理, 并且在blob存储中维护负载引用的映射
  │       │   └───────────────┘    * 集成区块链接口 [Bi] 插件
  │       │
  │       │   ┌───────────────┐  - 从共享存储中下载
  │       ├───┤ shared    [Sd]│    * 并行异步下载
  │       │   │ download      │    * 弹性重试和异步恢复
  │       │   └───────────────┘    * 完成后通知事件聚合器
  │       │
  │       │   ┌───────────────┐
  │       ├───┤ identity [Im] │  - 跨组件中心化身份管理服务
  │       │   │ manager       │    * 解决API输入 identity + key 组合 (短名称, 格式等)
  │       │   │               │    * 解决将已注册的链上签名返回身份
  │       │   └───────────────┘    * 集成区块链接口和可插拔身份接口（TBD）
  │       │
  │       │   ┌───────────────┐  - 通过插件跟踪对外部组件执行的所有操作
  │       ├───┤ operation [Om]│    * 以输入、输出更新数据库
  │       │   │ manager       │    * 提供跨插件的一致性重试语义
  │       │   └───────────────┘
  │       │
  │       │   ┌───────────────┐  - 私有数据管理和验证
  │       ├───┤ event     [Em]│    * 实现批量组件的分发
  │       │   │ manager       │    * 集成数据交换 [Dx] 插件
  │       │   └──────┬────────┘    * 集成区块链接口 [Bi] 插件
  │       │          │
  │       │   ┌──────┴────────┐  - 控制外部数据接受
  │       │   │           [Ag]│    * 集成数据交换 [Dx] 插件
  │       │   │ aggregator    │    * 集成共享数据接口 [Ss] 插件
  │       │   │               │    * 集成区块链接口 [Bi] 插件
  │       │   │               │  - 确保只有所有数据都可用时有效事件才会被分发
  │       │   └──────┬────────┘    * 上下文感知，避免“世界阻塞”场景的出现
  │       │          │
  │       │   ┌──────┴────────┐  - 订阅管理
  │       │   │           [Sm]│    * 创建和管理订阅
  │       │   │ subscription  │    * 创建和管理订阅
  │       │   │ manager       │    * 发消息到事件匹配逻辑
  │       │   └──────┬────────┘
  │       │          │
  │       │   ┌──────┴────────┐  - 管理事件到已连接应用的传递
  │       │   │ event     [Ed]│    * 集成数据交换 [Dx] 插件
  │       │   │ dispatcher    │    * 集成区块链接口 [Bi] 插件
  │       │   └───────────────┘
  │       │
  │       │   ┌───────────────┐  - Token 创建、传输的初始化、索引和协同
  │       ├───┤ asset     [Am]│    * 可替代 Token: 数字化价值/结算 (coins)
  │       │   │ manager       │    * 不可替代 tokens: NFTs / globally uniqueness / digital twins
  │       │   └───────────────┘    * 交易历史的全索引
  │       │   [REST/WebSockets]
  │       │   ┌─────┴─────────────┐   ┌──────────┐   ┌─ 
  │       │   │ ERC-20 / ERC-721  ├───┤ ERC-1155 ├───┤  创建 token 连接器的简单框架
  │       │   └───────────────────┘   └──────────┘   └─ 
  │       │
  │       │   ┌───────────────┐
  │       ├───┤ sync /   [Sa] │  - 同步、异步桥接
  │       │   │ async bridge  │    * 提供同步请求、应答APIs
  │       │   │               │    * 转换为底层事件驱动API
  │       │   └───────────────┘
  │       │
  │       │   ┌───────────────┐  - 聚合报文和数据，并汇总hash值以进行固定
  │       ├───┤ batch     [Ba]│    * 可插拔分发器
  │       │   │ manager       │  - 数据库和主线API处理分离
  │       │   │               │    * 有关主动、主动排序的更多信息，请参考架构图
  │       │   └──────┬────────┘  - 管理批处理器的创建
  │       │          │
  │       │   ┌──────┴────────┐  - 短暂的代理启动以按需组装批次
  │       │   │ batch     [Bp]│    * 与作者+消息类型耦合
  │       │   │ processor     │  - 批量构建100多条信息以实现高效固定
  │       │   │               │    * 聚合消息和数据，并汇总hash值以进行固定
  │       │   └───────────────┘  - 配置失效后自动关闭
  │       ... more TBD
  │
Plugins: 每个插件都包含一个Go shim，以及一个远程代理微服务运行环境(有需要的话)
  │
  │           ┌───────────────┐  - 区块链接口
  ├───────────┤           [Bi]│    * 交易提交 - 包括签名秘钥管理
  │           │ blockchain    │    * 时间监听
  │           │ interface     │    * 标准化操作，和自定义链上耦合
  │           └─────┬─────────┘
  │                 │
  │                 ├─────────────────────┬───────────────────┬-───────────────────┐
  │           ┌─────┴─────────┐   ┌───────┴───────┐   ┌───────┴────────┐   ┌───────┴────────┐
  │           │ ethereum      │   │ fabric        │   │ corda/cordapps │   │ tezos          │
  │           └─────┬─────────┘   └───────────────┘   └────────────────┘   └────────────────┘
  │           [REST/WebSockets]
  │           ┌─────┴────────────────────┐   ┌────────────────────────┐   ┌─ 
  │           │ transaction manager [Tm] ├───┤ Connector API [ffcapi] ├───┤ 构建区块链连接器的简单框架
  │           └──────────────────────────┘   └────────────────────────┘   └─ 
  │        
  │           ┌───────────────┐  - Token 接口
  ├───────────┤ tokens    [Ti]│    * 标准化的核心概念：token pools，transfers，approvals
  │           │ interface     │    * 可插拔的跨 token 标准
  │           └───────────────┘    * 通过微服务连接器针对自定义 token 标准的简单实现方式的支持
  │           [REST/WebSockets]
  │           ┌─────┴─────────────┐   ┌──────────┐   ┌─ 
  │           │ ERC-20 / ERC-721  ├───┤ ERC-1155 ├───┤  构建 token 连接器的简单框架
  │           └───────────────────┘   └──────────┘   └─ 
  │
  │           ┌───────────────┐  - P2P 内容寻址文件系统
  ├───────────┤ shared    [Si]│    *  负载 上传 / 下载
  │           │ storage       │    * 有效的负载参考管理
  │           │ interface     │
  │           └─────┬─────────┘
  │                 │
  │                 ├───────── ... 可拓展至任意共享存储系统，可供所有成员访问
  │           ┌─────┴─────────┐
  │           │ ipfs          │
  │           └───────────────┘
  │
  │           ┌───────────────┐  - 私有数据交换
  ├───────────┤ data      [Dx]│    * Blob 存储
  │           │ exchange      │    * 私有加密消息传递
  │           └─────┬─────────┘    * 加密文件传输
  │                 │
  │                 ├─────────────────────┬────────── ... 可拓展至任意私有数据交换技术
  │           ┌─────┴─────────┐   ┌───────┴───────┐
  │           │ https / MTLS  │   │ Kaleido       │
  │           └───────────────┘   └───────────────┘
  │
  │           ┌───────────────┐  - API 认证和认证接口
  ├───────────┤ api auth  [Aa]│    * 验证加密凭证 (OpenID Connect id token JWTs etc.)
  │           │               │    * 提取 API/user 身份 (用于身份接口映射)
  │           └─────┬─────────┘    * 细粒度 API 访问控制的执行点
  │                 │
  │                 ├─────────────────────┬────────── ... 可拓展到任意单点登录技术
  │           ┌─────┴─────────┐   ┌───────┴───────┐
  │           │ apikey        │   │ jwt           │
  │           └───────────────┘   └───────────────┘
  │
  │           ┌───────────────┐  - 数据库集成
  ├───────────┤ database  [Di]│    * 创建, 读取, 更新, 删除 (CRUD) 操作
  │           │ interface     │    * 过滤和更新定义接口
  │           └─────┬─────────┘    * 迁移和索引
  │                 │
  │                 ├───────── ... 可拓展至任意NoSql (CouchDB / MongoDB etc.)
  │           ┌─────┴─────────┐
  │           │ sqlcommon     │
  │           └─────┬─────────┘
  │                 ├───────────────────────┬───────── ... 可拓展至其他SQL数据库
  │           ┌─────┴─────────┐     ┌───────┴────────┐
  │           │ postgres      │     │ sqlite3        │
  │           └───────────────┘     └────────────────┘
  │
  │           ┌───────────────┐  - 将核心事件引擎连接至外部框架和应用
  ├───────────┤ event     [Ei]│    * 支持长周期 (可持续的) 和短暂的事件订阅
  │           │ interface     │    * 批量、过滤，所有的传输都会在核心区域进行处理
  │           └─────┬─────────┘    * 接口支持连接输入 (websocket) 和连接输出 (代理运行环境插件) 插件
  │                 │
  │                 ├───────────────────────┬──────────   ... 可拓展至其他的事件总线 (Kafka, NATS, AMQP etc.)
  │           ┌─────┴─────────┐     ┌───────┴────────┐
  │           │ websockets    │     │ webhooks       │
  │           └───────────────┘     └────────────────┘
  │  ... 更多 TBD

  额外的工具类框架
              ┌───────────────┐  - REST API 客户端
              │ rest      [Re]│    * 提供便利性和日志
              │ client        │    * 标准的认证, 配置和重试逻辑
              └───────────────┘    * 构建和重试

              ┌───────────────┐  - WebSocket 客户端
              │ wsclient  [Wc]│    * 提供便利性和日志
              │               │    * 标准化认证，配置和重连逻辑
              └───────────────┘    * 构建于 Gorilla WebSockets

              ┌───────────────┐  - 翻译框架
              │ i18n      [In]│    * 所有翻译内容必须要添加到 `en_translations.json` - 用 `FF10101` 作为 key
              │               │    * 错误会被打包, `error` 包内提供了额外的特性 (堆栈等.)
              └───────────────┘    * 也支持描述翻译, 例如 OpenAPI 描述

              ┌───────────────┐  - 日志框架
              │ log       [Lo]│    * 日志框架 (logrus) 集成了上下文标签
              │               │    * 上下文贯穿整个代码，用于传递API调用的上下文件,  以及日志上下文
              └───────────────┘    * 样例: 所有的API调用都有着他随身的IP, 以及时长

              ┌───────────────┐  - 配置
              │ config    [Co]│    * 基于日志框架的文件和环境变量 (viper)
              │               │    * 主要配置key全部集中定义
              └───────────────┘    * 插件通过返回其配置结构进行集成 (JSON 标签)

```
