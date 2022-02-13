sh Wi-Fi


1. Introduction
2. How to check whether AP BSS supports 802.11k, 802.11v and 802.11r with Wireshark
3. References


# **Introduction**
Mesh Wi-Fi 最主要跟基本的訴求就是提供零死角、好網路品質的無線網路環境。Mesh Wi-Fi 的 Network Topology 如下圖右一。
![](https://i.imgur.com/PxusH2w.jpg)


End Node 為 Wi-Fi AP，此種拓樸可以增加無線網路覆蓋率與 Wi-Fi 訊號。Client 可視當下情況選擇合適的 AP 使用。例如，當 Client 從 A 地移動 B 地，Client 可以自動從原本連線的 AP，換成附近的 AP，確保連線品質不會斷線。這種 Client 自動切換 AP 的功能稱之為"Roaming"。Roaming 是構成 Mesh Wi-Fi 其中一項基本功能。

接下來，我們介紹 Mesh Wi-Fi 中，會使用到的三種技術 IEEE 802.11k、 802.11v、802.11r。


# 802.11k  (Roaming)




**802.11v ( + 802.11k = Mesh)**

