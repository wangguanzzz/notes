## 网络

除了有VPC， ali cloud 还有一个网络叫“经典网络”

## ECS
弹性裸金属 - 融合了物理机和云服务器各自的优势
* 高CPU需求
* 在上面再做虚拟化

## 云块存储
* 实例操作系统块 不加密

SSD共享块存储，用于集群 （类似于EFT）

系统盘上限都是500G

## VPC
* overlay 技术，分隔开物理网络和虚拟网络 , 虚拟网络平面， 虚拟网络，虚拟路由器
* VXLAN， 在overlay的基础上，将二层报文在三层进行扩展， 每个VPC有一个隧道的ID, 在三层报文中，确保VPC 隔离
* vswitch, vrouter都是默认自带的

## VPC组件
没有internet gateway, 只需要有公网IP ，就可以上网
云企业网 - 融合VPC, vpn , 直连，跨区域，形成企业网络 （类似 transitgateway）
VPN 网关


## deploy NFT
* metamask
* ipfs desktop
* etf remix https://remix.ethereum.org