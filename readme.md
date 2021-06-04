# 로라모듈을 이용한 화재 대피 경로안내 시스템

## 기술스택
<img src="https://img.shields.io/badge/json-000000?style=flat-square&logo=json&logoColor=white&link=https://ko.wikipedia.org/wiki/JSON"> <img src="https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=Grafana&logoColor=white&link=https://grafana.com"> <img src="https://img.shields.io/badge/InfluxDB-22ADF6?style=flat-square&logo=InfluxDB&logoColor=white&link=https://grafana.com"> <img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=Linux&logoColor=white&link=https://grafana.com"> <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white&link=https://grafana.com"> <img src="https://img.shields.io/badge/mqtt-3C5280?style=flat-square&logo=Eclipse Mosquitto&logoColor=white&link=https://grafana.com">

## Intro

해당 프로젝트는 스마트시티에서 활용가능한 LoRaWAN을 이용하여 화재 발생 시 건물 내의 인원들에게 최적의 대피 경로를 제공하는 시스템을 설계한다. 

<img src="/img/Architecture.png" width="400px" height="200px"></img><br/>

## 단말 부
## YoLo(Jetson Nano)
YOLO 객체 탐지를 위해 Jetson Nano에 CSI 카메라 센서를 부착하여 건물 내 인원수와 화재 발생 여부를 파악하였다. YOLO의 구동은 아래 사이트들을 참조하여 진행하였다.

#### 동영상 분석을 위한 OPENCV 설치
https://ultrakid.tistory.com/10

#### 젯슨나노에 darknet project 설치
https://wendys.tistory.com/143

#### YOLO 학습을 위한 이미지 크롤링
https://velog.io/@banana/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9C%BC%EB%A1%9C-%ED%85%8D%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EC%9D%B4%EB%AF%B8%EC%A7%80-%ED%81%AC%EB%A1%A4%EB%A7%81%ED%95%98%EA%B8%B0

#### YOLO 학습을 위한 이미지 라벨링
https://junyoung-jamong.github.io/deep/learning/2019/01/22/Darkflow%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%B4-YOLO%EB%AA%A8%EB%8D%B8-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%94%94%ED%85%8D%EC%85%98-%EA%B5%AC%ED%98%84-in-windows.html

https://github.com/tzutalin/labelImg

## Raspberry Pi
Raspberry Pi 3 Model B에 온도 센서(DHT-11)를 연결하여 온도를 통해 화재 상황 감지를 보조한다. 또한 LED모듈을 부착하여 서버로부터 제공받은 데이터로 대피 경로를 안내한다. 각 디바이스에 LoRa 통신 모듈 SX1276을 결합하여 서버부와 데이터를 송수신한다.

#### 아래는 온도 센서 라이브러리 설치 명령어이다.
<pre>
<code>
$ git clone https://github.com/adafruit/Adafruit_Python_DHT.git
$ cd Adafruit_Python_DHT
$ sudo apt install build-essential python-dev python-openssl
$ sudo python setup.py install
</code>
</pre>

Lora 통신 방법은 아래에서 설명한다.

## 서버 부
## LoRa 통신환경 구축
LPWAN 기술 중에서도 LoRa를 이용한 통신은 약 5~10km의 장거리에서도 통신이 가능하고, 저전력 환경에서도 작동이 가능하므로 스마트시티 환경에 적합하기 때문에 이를 이용하여 시스템을 구현하였다. keronet에서 제공하는 https://scienceiot.kreonet.net/ 망을 이용하기 위해 라즈베리파이 3B를 사용하였으며, 클래스는 A 클래스를 사용하였다.

#### 다음은 lora 송신 코드를 실행하는 명령어이다.
<pre>
<code>
$ git clone https://github.com/CNU-T9/2021_Capstone.git
$ cd 2021_Capstone
$ python simpletranv4.py
</code>
</pre>


## mqtt
LoRa 망에 저장되어있는 데이터를 수집하고 사용하기 위해 Topic을 MAC 주소별로 publish한다.

<pre>
<code>
$sudo apt-get install mosquitto-clients
</code>
</pre>

## 사용자 부
InfluxDB에 저장된 데이터를 실시간으로 모니터링하기 위해 대시보드인 Grafana를 이용해 화면에 출력한다.

예시)

<img src="/img/grafana.jpg" width="400px" height="300px"></img><br/>







