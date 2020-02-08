# Mesh Wi-Fi


# **Introduction**
Mesh Wi-Fi 最主要跟基本的訴求就是提供零死角、好網路品質的無線網路環境。Mesh Wi-Fi 的 Network Topology 如下圖右一。
![](https://i.imgur.com/PxusH2w.jpg)


End Node 為 Wi-Fi AP，此種拓樸可以增加無線網路覆蓋率與 Wi-Fi 訊號。Client 可視當下情況選擇合適的 AP 使用。例如，當 Client 從 A 地移動 B 地，Client 可以自動從原本連線的 AP，換成附近的 AP，確保連線品質不會斷線。這種 Client 自動切換 AP 的功能稱之為"Roaming"。Roaming 是構成 Mesh Wi-Fi 其中一項基本功能。

接下來，我們介紹 Mesh Wi-Fi 中，會使用到的三種技術 IEEE 802.11k、 802.11v、802.11r。


* **802.11k (Radio Resource Management)**
802.11k 標準可協助 Client 建立最佳化頻道列表，加速搜尋附近可用的 AP 漫遊目標。當現有的 AP 訊號強度減弱時，STA 會送出 Neighbor Report Request 給予目前關聯的 AP。AP 會回復Neighbor Report Response，此回覆會含有建議此 STA 的其他 AP 候選人。STA 可根據此列表選擇下一個要漫遊的對象。

![](https://i.imgur.com/3rQ2NLO.jpg)

![](https://i.imgur.com/GDVzufJ.jpg)

* **802.11v (BSS Transition)**
AP 可以使用 BSS 轉換管理功能 (802.11v) 將周遭 AP 的載入資訊提供給 Client 的控制層，以影響 Client 漫遊行為。 Client 在決定可能的漫遊目標時，會將此資訊列入考量。


* **802.11r (Fast Transition)**
當 Client 在同一個網路的 AP 之間漫遊時，802.11r 會利用一種稱為「快速基本服務設定轉換」（Fast Basic Service Set Transition，FT）的功能，進行更快速的認證。FT 可以同時與預先分享密鑰（PSK）和 802.1X 認證方法搭配使用。



基本上使用 802.11k + 802.11v 即可達到 Mesh。當使用者一起使用 802.11k 和 802.11v 可快速搜尋最佳目標 AP 的功能，再加上 FT 更快的 AP 關聯方法 (802.11r) 結合時，以便進一步達到 Seamless Roaming，讓使用者獲得更棒的 Wi-Fi 體驗。




# How to check whether AP BSS supports 802.11k, 802.11v and 802.11r with Wireshark


* **802.11k (Radio Resource Management)**
![](https://i.imgur.com/pbJ5OsY.jpg)


* **802.11v (BSS Transition)**
![](https://i.imgur.com/8DgZL5p.png)



* **802.11r (Fast Transition)**
![](https://i.imgur.com/Rc4NlNk.png)





# **References**

