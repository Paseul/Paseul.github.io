---
title: 5G 국제 표준의 이해
layout: single
read_time: true
comments: true
related: true
categories: Networks
tags:
- Networks
---

본 포스팅은 삼성에서 제공한 [5G 국제 표준의 이해](https://images.samsung.com/is/content/samsung/p5/global/business/networks/insights/white-paper/who-and-how_making-5g-nr-standards/who-and-how_making-5g-nr-standards_KR.pdf)를 요약 정리한 포스팅 입니다
## 5G NR(New Radio) 표준
### **표준**
표준(Standardization)을 통하여 특정 국가나 사업자에게만 특화된 제품이 아니라 전세계에서 통용되는 제품을 생산할 수 있게 된다.
과거에는 국가별, 제조사별, 표준화 단체별로 제품을 개발하여 시장을 많이 점유한 기술이 사실상(De-facto) 국제표준이 되기도 했으나 최근에는 이러한 규격을 표준화하여 국제적으로 합의된 국제 표준을 사용하고 있다.<br>

|국제 표준을 통해|R&D 비용 절감|소비자는|
|:---:|:---:|:---:|
|통신기능 주체간에 정보교환 및 신호 처리가 가능|제조사는 대량생산, 중복투자 방지, 기술이전, 가능|제조사간의 경쟁을 통해 가성비 높은 제품을 확보|

<br>

**Generation 구분:**  각 세대를 구분 짓는 것은 새로운 서비스가 아니라, 그런 서비스를 가능하게 한 **'기술적 혁신'** 새로운 이동통신 기술 혁신들은 바로 각 세대의 **"표준"**에 의해 정의되어 왔다.<br>
### **5G 표준**
<center><img src="/assets/images/Networks/ITU-3GPP_relationship.PNG" width="60%" ></center>

**ITU(국제 전기 통신 연합, International Telecommunication Union)** 에서 비전 및 목표를 제시하고  **국제 표준화 단체(3GPP)** 에서 기술 표준 개발
**3GPP**는 전세계 이동통신 사업자, 장비 제조사, 단말 제조사, 칩 제조사 및 세계 각국의 표준화 단체와 연구기관 등 약 500여개 업체가 참여하는 최대 국제 이동통신 표준화 단체<br>
**5G 표준의 특징**
1) 초 광대역 서비스 (eMBB: enhanced Mobile Broadband)
: 사용자당 100Mbps에서 최대 20Gbps까지 훨씬 빠른 데이터 전송속도 제공을 목표
2) 고신뢰/초저지연 통신 (URLLC: Ultra Reliable & Low Latency Communications)
: 기존 수십 밀리 세컨드 (1ms = 1/1000 초) 걸리던 지연 시간을 1ms 수준으로 최소화하는 것을 목표
3) 대량연결 (mMTC: Machine-Type Communications)
: 1 km2 면적 당1백만개의 연결(connection)을 지원하는 것을 목표

<center><img src="/assets/images/Networks/4Gvs5G.PNG" width="70%" ></center>
*4G LTE도 초기 상용화 시점(2010년 경) 당시 최대속도가 75Mbps였고 2018년에야 목표성능 1Gbps 상용이 가능 했고 5G도 마찬가지일 것으로 예상된다*<br>
*4G의 주파수 대역은  6GHz 이하(Below 6GHz)였지만 28GHz와 39GHz 등 밀리미터파(mmWave)로 불리는 초고주파 대역(Above 6GHz)까지 함께 사용하게 된다*<br>

* **빔 포밍 기술:** 많은 수의 안테나에 실리는 신호를 각각 정밀하게 제어하여 특정 방향으로 에너지를 집중시키거나 또는 반대로 특정 방향으로는 에너지가 가지 않도록 조절이 가능한 기술. 전파의 에너지를 집중시켜 거리를 늘리고 빔(Beam)간에는 간섭을 최소화 시킬 수 있다. 이렇게 예리한 빔을 계속 정확하게 추적(tracking) 해야 하는 것이 기술적 관건이 된다.
<center><img src="/assets/images/Networks/Beamforming.PNG" width="80%" ></center>
* **Massive MIMO:** 수 많은 안테나 배열(Massive Antenna Array)을 활용하여 같은 무선 자원을 여러명이 동시에 사용하는 기술. 4G에서의 MIMO기술은 1차원(1D) 안테나 배열을 사용하였기 때문에 자유도(degree of freedom)가 낮아 수평방향(horizontal)사용자만 구분 했지만 5G에서는 수십개 이상의 안테나를 2차원(2D)으로 배치해 수직-수평(horizontal & vertical)방향 모두 사용자를 구분할 수 있다.
<center><img src="/assets/images/Networks/MassiveMIMO.PNG" width="80%" ></center>
* **Network Slicing:** 4G에서는 Voice와 Data 서비스로 구분해서 Voice에 대해서만 별도의 Qos(Quality-of-Service)를 제공했지만, 5G에서는 네트워크 슬라이싱을 통해 각각의 Data 서비스들도 독립적인 네트워크 자원 할당이 가능하고 따라서 각 서비스별로 다른 서비스의 영향을 받지 않으면서 품질을 보장할 수 있다. **특히, 시간지연에 민감한 서비스(Mission Critical Service)들의 품질을 보장할수 있게 되어 이동통신 사업자는 특화 서비스에 대한 별도의 과금체계도 도입할 수 있다.**
<center><img src="/assets/images/Networks/NetworkSlicing.PNG" width="80%" ></center>
* **NSA와 SA:** NAS(Non-Standalone)는 초기 상용망에 구현괄 것으로 예상되는 구조로, 단말의 이동성(mobility) 관리 등을 제어하는 제어 플레인(control plane)의 동작은 4G LTE 망을 활용하면서 사용자 플레인(User plane/Data plane)에 해당하는 데이터 트래픽은 5G 망으로 주고 받는다. SA(Standalone) 구조는 제어 채널이나 데이터 채널 모두 5G의 자체구조를 사용하는 구조이다.
<center><img src="/assets/images/Networks/NSA-SA.PNG" width="80%" ></center>
* **표준의 Release 개념:** **Release는 3GPP의 시간 스케쥴이다.** 이동통신 국제 표준은 단기간에 1회성으로 정해지는 것이 아니며 장기간에 걸쳐 수 차례 업그레이드 된다. 다음 세대가 나오더라도 수년정도는 이전 세대 기술이 계속 보완하여 발표(Release) 한다.
<center><img src="/assets/images/Networks/Release.PNG" width="80%" ></center>