# IEEE 802.11s, Mesh Wi-Fi
###### tags: `IEEE802.11` `Networking`

# **Introduction**
Mesh Wi-Fi 最主要跟基本的訴求就是提供零死角、好網路品質的無線網路環境。Mesh Wi-Fi 的 Network Topology 如下圖右一。
![](https://i.imgur.com/PxusH2w.jpg)

End Node 為 Wi-Fi AP，此種拓樸可以增加無線網路覆蓋率與 Wi-Fi 訊號。Client 可視當下情況選擇合適的 AP 或 Band 使用。例如，當 STA 從 A 地移動 B 地，STA 可以自動從原本連線的 AP，換成附近的 AP;STA 從原本 AP 的5G Wireless 連線，切換成 2.4G wirless 連線，確保連線品質不會斷線，

接下來，我們介紹 Mesh Wi-Fi 中，會使用到的三種技術 IEEE 802.11k、 802.11v、802.11r。

---

* **802.11k (Radio Resource Management)**

802.11k enable STAs to understand the radio environment in which they exist.

RRM includes the following
- Negighbor Report Request/Response
- Measurement Report/Response

802.11k support Negighbor report IE. STAs can send out Neighbor Report Request to current servicing AP. AP will reply Neighbor Report Response to STAs. it has many candidate APs in Neighbor Report Response.STA can select the next AP according to candidate APs.

![](https://i.imgur.com/3rQ2NLO.jpg)

![](https://i.imgur.com/GDVzufJ.jpg)


802.11k also supports Measurement Requests, found using a wlan.rm.action_code == 0 filter. The screenshot below shows Orbi issuing a Beacon request. A more specific filter would be wlan.tag.number == 38, found under Tagged Parameters.

![](https://i.imgur.com/QESvdUu.png)

If we go looking for a Measurement Report by filtering for wlan.tag.number == 39, however, we don't find one in this capture, or any others for the Intel STA. But looking at a capture using the Samsung Tab STA, shows two responses. However, the report details says no measurement was actually made as indicated by measurement duration: 0x0000 and all the other zero values. This is an example of a feature that appears to be supported, but actually isn't because no actionable information is provided.

![](https://i.imgur.com/PbgP8Mg.png)




---

* **802.11v (Wireless Network Management)**

Wireless Network Management (WNM) enables STAs to exchange information for the purpose of improving the overall performance of the wireless network.

WNM includes the following
- BSS transition management

AP 可以使用 BSS transition management 將周遭 AP 的載入資訊提供給 Client 的控制層，以影響 Client 漫遊行為。 Client 在決定可能的漫遊目標時，會將此資訊列入考量。

![](https://i.imgur.com/hqitzDK.png)

---

* **802.11r (Fast Basic Service Set (BSS) Transition)**

A station (STA) movement that is from one BSS in one extended service set (ESS) to another BSS within the same ESS and that minimizes the amount of time that data connectivity is lost between the STA and the distribution system (DS).

當 Client 在同一個網路的 AP 之間漫遊時，802.11r 會利用一種稱為「快速基本服務設定轉換」（Fast Basic Service Set Transition，FT）的功能，進行更快速的認證。FT 可以同時與預先分享密鑰（PSK）和 802.1X 認證方法搭配使用。

![](https://i.imgur.com/dziYoVQ.png)

---


基本上使用802.11k即可達到Roaming。當使用者一起使用802.11k和 802.11v可快速搜尋最佳目標AP的Roaming功能。此時再加上802.11r更快的AP關聯方法(Fast Transition)結合時，以便進一步達到Seamless Roaming，讓使用者獲得更棒的 Wi-Fi 體驗。




# How to check whether AP BSS supports 802.11k, 802.11v and 802.11r with Wireshark


* **802.11k (Radio Resource Management)**
![](https://i.imgur.com/oh5eH8H.jpg)



* **802.11v (BSS Transition)**
![](https://i.imgur.com/8DgZL5p.png)



* **802.11r (Fast Transition)**
![](https://i.imgur.com/Rc4NlNk.png)





# **References**
[Wi-Fi Roaming Secrets Revealed - Part 2](https://www.smallnetbuilder.com/wireless/wireless-features/33196-wi-fi-roaming-secrets-revealed-part-2)

[IEEE 802.11s](https://en.wikipedia.org/wiki/IEEE_802.11s)
