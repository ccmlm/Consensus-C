# Consensus C

|算法|类别<sup>*1*</sup>|VRF|代表项目|语言|建议开发模式|移植成本<sup>*2*</sup>|维护成本|灵活性|理论性能|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|EC|-|**有**|Filecoin|Go(lotus)|延用官方开发模式: Rust 开发 zkp 库, Go 通过 FFI 调 Rust|-|-|-|-|
|DPOS|BFT|无|EOS|C++|-|高|高|-|**优+**|
|**Tendermint**|BFT|无|Cosmos|Go|参考 filecoin(lotus): Rust 开发 zkp 库, Go 通过 FFI 调 Rust|**低**|**低**|劣|中|
|Aura +</br>Grandpa|batch BFT|无|Polkadot|Rust|使用整个 substrate|中|中|劣|**优+**|
|Aura +</br>Grandpa|batch BFT|无|Polkadot|Rust|从 substrate 中抽取共识与网络组件|高|中|**优**|**优+**|
|**Babe +**</br>**Grandpa**|batch BFT|**有**|Polkadot|Rust|使用整个 substrate|中|中|劣|**优**|
|**Babe +**</br>**Grandpa**|batch BFT|**有**|Polkadot|Rust|从 substrate 中抽取共识与网络组件|高|中|**优**|**优**|
|POW +</br>Grandpa|batch BFT|无<sup>*3*</sup>|Polkadot|Rust|使用整个 substrate|中|中|劣|**优-**|
|POW +</br>Grandpa|batch BFT|无<sup>*4*</sup>|Polkadot|Rust|从 substrate 中抽取共识与网络组件|高|中|**优**|**优-**|
|FBA(SCP)|federated BFT|无<sup>*5*</sup>|Stellar|C++|Rust 通过 FFI 调 C++ 完成共识|高|高|**优**|**优+**|
|SPOS|sharding BFT|**有**|Elrond|Go|参考 filecoin(lotus): Rust 开发 zkp 库, Go 通过 FFI 调 Rust|高|中|良|**优+**|
> 注释 1: 此处的类别指共识过程中的投票模式    
> 注释 2: '低' 表示初次上线预计耗时 1 周, '中' 4 周, '高' 6 周    
> 注释 3/4: POW 出块具有随机性, 不依赖于 VRF, 但小规模的网格应用 POW 并不安全    
> 注释 5: 理论上 FBA(SCP) 的出块顺序难以预测，其随机性不依赖于 VRF

**综上, 最优'候选解'有三:**

1. Tendermint
  - 技术成本低
  - 预计 1 周上线
  - ! 无 VRF
  - ! 性能中等
2. 全套 subtrate(Babe + Grandpa)
  - ! 技术成本中等
  - ! 预计 4 周上线
  - 有 VRF
  - 性能优秀(网络结论,未实测)
3. Babe + Grandpa + Libp2p
  - ! 技术成本中等
  - ! 预计 6 周上线
  - 有 VRF
  - 性能优秀(网络结论,未实测)
  - 灵活性强, 利于自主定制，利于长期维护

<方案 1> 最便捷, <方案 3> 最合理，<方案 2> 折中.

**建议选型: <方案 3>.**
