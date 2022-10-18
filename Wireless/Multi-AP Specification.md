# Multi-AP Specification - Brief
###### tags: `EasyMesh` `IEEE802.11`

EasyMesh R1
> R1是定義一個基本的共通性的Mesh網路框架，其他主要功能有：
>
> * 診斷功能，能監控接入點和無線用戶端兩者的連線能力、回程(Backhaul)或>前傳(Fronthaul)的效能及信標(Beacon)報告
> * 引導功能，無線AP會時時地進行效能比對及監控連線品質，將裝置引導到較好的無線環境。
> * 頻段及訊號強度的配置優化
> * 透過訊息交換來強化引導功能


EasyMesh R2
> 主要有以下幾項功能的加強，流量控制、安全性、使用頻段的優化、更進階的診斷功能，和採用雲端為基礎的遠端管理方法
> 
> * 流量控制，如QOS功能的加強，流量分流、強化無線用戶的引導體驗
> * 安全性，強化多個接入點的認證機制和啟用、IEEE 1905訊息的加密
> * 使用頻段的優化，增強DFS頻段的使用便利性
> * 進階診斷功能，強化頻段掃描機制，採用更聰明的訊息交換機制以利使用者得到較好的體驗，回程(Backhaul)效能的診斷

EasyMesh R3
>計畫將會含有以下功能：
>
> * 避免多播迴圈機制
>
> * 可設定服務的優先權
>
> * 支持Wi-Fi 6
>
> * 分層安全性
>
> * 支援Wi-Fi Easy Connect(任何的Wi-Fi設備都可以透過掃描QR Code的方式安全地連接到一個Wi-Fi網路)


#### 3.1.3 Definitions

![](https://i.imgur.com/w0glwed.jpg)
![](https://i.imgur.com/LjQHAii.jpg)
![](https://i.imgur.com/6M4bjZI.jpg)

### 4.1 Multi-AP architecture
A Multi-AP network consists of two types of logical entities:
* One Multi-AP Controller and
* One or more Multi-AP Agents

Multi-AP Agent can be divided into for two kinds role further:
* Multi-AP Registrar
* Multi-AP Enrollee

Multi-AP Registrar is the Multi-AP Agent of Multi-AP Controller. If no specific reference, Multi-AP Agent is meaning Multi-AP Enrollee.



#### 4.1.1 Multi-AP example deployment

![](https://i.imgur.com/B9v46NA.png)

![](https://i.imgur.com/Mxk7u05.png)



### 5.2 PBC Backhaul STA onboarding procedure
Exactly, Multi-AP Registrar and Multi-AP Enrollee are in charge of onboarding procedure. No need Multi-AP Controller involve it. 

![](https://i.imgur.com/EmEpGG3.png)

## 8 Channel selection


## 11 Client steering
Multi-AP control messages enable efficient steering of STAs between BSSs in a Multi-AP network. These control messages enable steering of client STAs which support <span class="red">802.11v BSS Transition Management (BTM)</span> as well as <span class="red">client STAs which do not support BTM</span>.

## References
Multi-AP Specification Version 2.0




<style>
.red {
  color: red;
}
</style>