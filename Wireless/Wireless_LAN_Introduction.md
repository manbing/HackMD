# Wireless LAN Introduction
###### tags: `IEEE802.11` `Networking`


## Signal-to-Noise Ratio (SNR)
  802.11ac的AP可以支援BPSK、QPSK、16-QAM、64-QAM及256-QAM等調變技術，而且在不同調變技術下，還可以支援1/2、2/3、3/4或5/6等不同的編碼方式，前先有提到無線網路中傳輸的單位叫做Symbol，不同的調變技術代表在一個Symbol可以傳輸的位元數，而編碼的方式決定有效的資料位元數，例如採用64-QAM調變技術的話，代表一個Symbol可以表示64種不同的振幅及相位的變化，依據這64種不同的變化，我們可以用它來表示6個位元的資訊，同理256-QAM調變技術則可以攜帶8個bit的資訊，採用3/4的編碼方式表示每4個位元的資料中有3個是有效的資料，另外一個位元用來檢查並回復錯誤的資料位元，所以資料的傳輸效率是3/4，同理在5/6的編碼方式在傳輸效率上會比3/4來得高，所以在表中的傳輸速度會因更好的調變技術及更複雜的編碼方式而逐步提高。

  無線網路的傳輸速度除了硬體(11a、11g、11n或11ac)跟使用的頻道寬度(20MHz、40MHz、80MHz或160MHz)有直接的關係外，先前的文章中有提到在802.11n跟802.11ac有MCS index這樣的概念，用戶端的設備選擇使用哪一個MCS Index，即代表使用哪一種調變技術跟不同的編碼方式，因此可以提供的最高連線速度就會不一樣，例如802.11n MCS 7，使用64-QAM的調變及5/6的編碼方式，如果設定使用40MHz及SGI的條件，連線速度為150Mbps， 802.11ac的MCS 9，使用802.11ac才支援的256-QAM的調變技術，搭配5/6的編碼，在支援一個spatial stream的手機或平板，使用80MHz及SGI的條件下，手機或平板跟無線基地台的連線速度為433.3Mbps.

  以一般交談的經驗來說，如果說話的音量不夠大，說話的速度又快的話，聽起來會是所有的話都糊在一起，不容易清楚的辨識每一個字，所以如果音量不夠大，我們就需要放慢說話的速度，這樣對方才能確認我們想要表達的每一個字，所以音量越大可以接受的說話速度越快，也表示能夠傳遞或溝通的訊息當然也就越多。

  在所有的環境中不同的頻道，都會有Noise Floor(這個名詞的中文翻譯還真多，簡單講就是背景的噪音)，如下圖所示，用戶端所在位置的無線訊號扣掉Noise Floor就是所謂的SNR(Signal-to-Noise Ratio)或稱之為信噪比，前面提到音量越大可以接受的說話速度越快，但是音量大小還是要跟環境的噪音做比較，環境的噪音越大時，我們需要適時的調高音量以維持一樣的說話速度，不然就需要降低說話速度以讓聽眾能夠清楚的聽到正確的內容。

  大部分環境的Noise Floor約在-92~94dBm間，實際的數值則需要特定的工具才能測量到精確的數值，筆電、平板或手機上面看到的都只是參考值，不要當真，如果手上有頻譜分析儀的話，它就能提供正確的數值。

  SNR不是絕對值，而是一個比較值，例如某一個無線終端設備接收到的訊號強度為-68dBm，而現場當時的Noise Floor是-92dBm，那麼SNR就是24dB，無線終端設備在選擇MCS index時的概念，跟我們前面舉的說話的例子一樣，SNR越高表示相對於背景噪音可以接收到的音量越大，可以使用的MCS index 當然越高，代表可以傳輸的速度越快，但SNR的數值跟MCS index之間有沒有轉換的公式?

  答案是沒有!但有專家分享關聯的表格(不是公式)，有興趣的朋友可以透過MCS跟SNR兩個關鍵字google一下就可以找到完整的資料，依據表格的內容，使用20MHz時，SNR需要31dB以上才能有802.11ac MCS9的傳輸速度，如果是40MHz時SNR需要34dB以上，這代表Noise Floor如果是-92dBm的地點，用戶端的訊號要有-58dBm，才能有200Mbps(1個Spatial Stream)或400Mbps (2個Spatial Streams)的連線速度，可以想像-58dBm蛋黃區不大，且沒有辦法穿透過磚牆或水泥牆


  設備接收訊號的靈敏度及MIMO的天線數，當然也會影響MCS index的選擇，訊號的靈敏度越高或是MIMO的天線數越多，相對於在同一個位置的低階設備會有較高MCS index的連線速度，另外，BER(Bit Error Rate > 10%)如果太高，表示傳輸的錯誤率太高，設備也會主動降低一層MCS index，以提高傳輸的效率，避免資料重傳造成傳輸媒介的浪費。

  所以標題所要表達的意思其實很簡單，訊號強度並不代表連線速度，連線速度也不代表傳輸效能，除了硬體的能力外，SNR及前面介紹過的同頻干擾、鄰頻干擾、非Wi-Fi的干擾源、用戶端的類型、應用或是設定的調整，才是影響傳輸效率最重要的因素，各位不妨想想，哪些是不可抗拒?哪些是可以控制的因素?我們才能知道無線區域網路設計的原則為何。


[傳輸速度的神 – SNR](http://www.purewifi.tw/?p=255)

## Radio Frequency Interference (RFI)
it is also as know as <span class="red">*noise*</span>.

### Co-Channel interference (CCI)

### Adjacent-Channel Interference (ACI)

### Not Wi-Fi interference


[無線網路效能的殺手之一: 同頻干擾](http://www.purewifi.tw/?p=116)
[無線網路效能的殺手之二: 鄰頻干擾](http://www.purewifi.tw/?p=126)


## Modulation and Coding Scheme (MCS)
設備接收訊號的靈敏度及MIMO的天線數，當然也會影響MCS index的選擇，訊號的靈敏度越高或是MIMO的天線數越多，相對於在同一個位置的低階設備會有較高MCS index的連線速度，另外，<span class="red">Bit Error Rate (BER)</span> 太高 ( > 10%)，表示傳輸的錯誤率太高，設備也會主動降低一層MCS index，以提高傳輸的效率，避免資料重傳造成傳輸媒介的浪費。

The AP’s radio will decode and demodulate the request. If no error occurs, it will send back an ACK with an MCS index supported by the client device. Every transmitter chooses which MCS to use.

[MCS Index Table, Modulation and Coding Scheme Index 11n, 11ac, and 11ax](https://mcsindex.com/)
[MCS Table and How To Use it](https://wlanprofessionals.com/mcs-table-and-how-to-use-it/)
[CWAP – HT/VHT Capabilities IE](https://mrncciew.com/2014/10/19/cwap-ht-capabilities-ie/)

## Bit Error Rate (BER)

## Spatial Stream
2x2 Antenna? MIMO?


##  Packet Error Rate (PER)





<style>
.green {
  color: #7FFF00;
}
</style>

<style>
.red {
  color: #FF5733;
}
</style>