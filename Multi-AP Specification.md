# Multi-AP Specification - Brief
###### tags: `IEEE802.11` `EasyMesh`

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