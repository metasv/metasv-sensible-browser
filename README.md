# SensibleToken 浏览器接口（MetaSV Based）


## 背景

第一版浏览器可以复用MetaSV提供的免费接口，加快开发速度，但是由于MetaSV的花费状态和交易历史采用的是裁剪模式（最近一个月的记录），因此这两部分内容由蒋杰提供。

接口按照分工可以分为以下3类：

1. MetaSV已经提供的基础接口（不包含任何sensible相关内容）（https://apiv2.metasv.com/）
2. 蒋杰提供交易input，output，P2PKH地址历史（域名待定）
3. MetaSV提供SensibleToken专用解析接口（https://apiv2.metasv.com/sensible/）

## 接口说明Spec

这里只列出浏览器所需要的接口。

接口统一使用openapi格式来进行定义，方便减少歧义以及生成sdk，可以在metasv的内测开发者平台上进行调试，https://sensible.metasv.com/。内测平台只供友商使用，包含一些未发布的新特性，请勿公开。



### 获取区块链状态

使用场景：首页，查看最新高度，最新区块，当前难度，累计工作量等

API接口：https://apiv2.metasv.com/block/info

API文档：https://sensible.metasv.com/#/block/get_block_info



### 获取区块列表（翻页）

使用场景：首页，翻页获取最新区块数据，包括区块头，交易数量，矿工，总手续费等等数据。

API接口：https://apiv2.metasv.com/block

API文档：https://sensible.metasv.com/#/block/get_block



### 获取区块详情

使用场景：区块详情页面，传入高度或者blockHash，查询block，显示该区块详细数据，数据结构与区块列表相同

API接口：https://apiv2.metasv.com/block/677344

API文档：https://sensible.metasv.com/#/block/get_block__blockId_



### 获取区块交易（翻页）

使用场景：区块详情页面，翻页获取该区块内的交易列表

API接口：https://apiv2.metasv.com/block/677344/txs

API文档：https://sensible.metasv.com/#/block/get_block__blockId__txs



### 获取交易详情

使用场景：交易详情页面，按txid查询交易，显示此交易的确认高度，手续费等详情

API接口：https://apiv2.metasv.com/tx/66741aaecb851746a767c8fb51f89b42b7e048ca2631212d7a46883b31108223

API文档：[https://sensible.metasv.com/#/tx/get_tx__txid_](https://sensible.metasv.com/#/tx/get_tx__txid_)

**注意**：这里解析出来的input和output是直接从生交易中解析出来的，input中不包含address和value，output不包含花费状态，浏览器可以无视



### 获取生交易

使用场景：交易详情页面，按txid查询交易，如果用户需要查询rawHex，就调用此接口

API接口：https://apiv2.metasv.com/tx/66741aaecb851746a767c8fb51f89b42b7e048ca2631212d7a46883b31108223/raw

API文档：https://sensible.metasv.com/#/tx/get_tx__txid__raw



### 获取交易input详情（蒋杰提供）

使用场景：交易详情页面，根据txid查询该交易所有input的详情，此接口携带address和value

API接口：https://apiv2.metasv.com/vin/66741aaecb851746a767c8fb51f89b42b7e048ca2631212d7a46883b31108223

API文档：[https://sensible.metasv.com/#/tx/get_vin__txid_](https://sensible.metasv.com/#/tx/get_vin__txid_)

**注意**：此接口MetaSV提供最近一个月的交易（不包括未确认交易），因此由蒋杰提供，接口路径和数据结构参考metasv接口。



### 获取交易output详情（蒋杰提供）

使用场景：交易详情页面，根据txid查询该交易所有output的详情，此接口携带output的花费状态

API接口：https://apiv2.metasv.com/vout/66741aaecb851746a767c8fb51f89b42b7e048ca2631212d7a46883b31108223

API文档：[https://sensible.metasv.com/#/tx/get_vout__txid_](https://sensible.metasv.com/#/tx/get_vout__txid_)

**注意**：此接口MetaSV提供最近一个月的交易（不包括未确认交易），因此由蒋杰提供，接口路径和数据结构参考metasv接口。



### 获取Address余额（只包含P2PKH）

使用场景：地址详情页面，根据address查询余额

API接口：https://apiv2.metasv.com/address/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/balance

API文档：https://sensible.metasv.com/#/address/get_address__address__balance



### 获取地址UTXO（翻页，只包含P2PKH）

使用场景：地址详情页面，如果用户需要查询地址的utxo，使用此接口

API接口：https://apiv2.metasv.com/address/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/utxo

API文档：https://sensible.metasv.com/#/address/get_address__address__utxo



### 获取Address交易历史（只包括P2PKH，蒋杰提供）

使用场景：地址详情页面，根据address获取p2pkh的交易历史，token的交易历史调用其他接口。

API接口：https://apiv2.metasv.com/address/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/tx

API文档：https://sensible.metasv.com/#/address/get_address__address__tx

**注意**：此接口MetaSV只提供最近一个月的交易（暂时不包括未确认交易），由蒋杰提供，接口路径和数据结构参考metasv接口


### 使用地址获取Sensible Utxo
使用场景：地址详情页，显示该地址所属的所有sensible token
API接口：https://apiv2.metasv.com/sensible/address/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/utxo
API文档：https://sensible.metasv.com/#/address/get_address__address__tx

### 使用genesis id取Sensible Utxo
使用场景：Genesis详情页，显示该Genesis所属的所有sensible token
API接口：https://apiv2.metasv.com/sensible/genesis/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/utxo
API文档：https://sensible.metasv.com/#/address/get_address__address__tx

### 使用地址获取Sensible tx
使用场景：地址详情页，显示该地址所属的所有sensible token
API接口：https://apiv2.metasv.com/sensible/address/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/tx
API文档：https://sensible.metasv.com/#/address/get_address__address__tx

### 使用genesis id取Sensible tx
使用场景：Genesis详情页，显示该Genesis所属的所有sensible token
API接口：https://apiv2.metasv.com/sensible/genesis/1JshWqkXRfq5DDYWqrQfm8Fun4rLmdqB1t/tx
API文档：https://sensible.metasv.com/#/address/get_address__address__tx