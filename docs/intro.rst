서론
====

원격시스템 모니터링은 전자장비들의 수가 늘어나고 그들의 관리가 필요해지기 시작한 시점부터 수요가 발생하였다. 전자장비들의 크기는 작아지고, 가격은 낮아지고 있기 때문에 더 많은 곳에 설치되고 더 많은 데이터들을 수집하여 원격지에 위치한 서버로 전달한다. 

기상 정보를 예로 들어 설명하자. 요즘도 군에서는 백엽상의 온도를 보고 
손으로 이 값을 기록하는지는 모르겠으나, 적어도 필자가 근무하던 시절에는 
그렇게 했었다. 하지만,  기상청에서는 좀 다르게 기상정보를 수집한다. 
1986년 아시안 게임과 1988년 올림픽 게임에 필요한 기상정보를 제공하기 
위해 경기장에 최초로 자동기상관측장비를 설치하였다. 하지만, 이 시점에는 
우리가 생각하는 중앙 집중형 관리시스템이 존재하지는 않았다. 1996년에 
들어서야 전국단위로 1분마다 정보를 수집하여 센터에서 통합정보를 볼 수 
있는 기상 연속관리시스템이 구축되었다.
(http://web.kma.go.kr/kma15/2004/contents/200403_05.htm) 
요즘은 동단위로 예보를 하는 
시대이니, 어디에 있는지는 잘 모르겠지만, 동마다 이런 관측장비가 다 
설치되어 있다고 봐야 할 것이다. 
아래 그림은 기상정보를 수집하는 과정이다.  

.. image:: _static/aws.png

기본적으로 네트워크에 연결되어 있을 수 밖에 없는 기지국, 라우터와 같은 통신 장비들은 snmp 프로토콜을 이용하여 초창기부터 원격관리가 가능하였다. snmp의 탄생적 특성상 주로 데이터의 입출력량을 모니터링할 수 있으며, UPS등의 전력시스템을 위한 전력 관련 정보들도 수집할 수 있다. NMS (Network Management System)는 snmp를 기본으로 통신장비들의 상황을 모니터링하고 제어할 수 있는 네트워크 통합 관리 시스템이다. 최근에 클라우드 서비스가 각광 받으면서 이 분야의 중요성도 부각되고 있다.

공장 자동화 분야에서도 본 글에서 논하고자하는 원격 시스템 모니터링이 사용된다. SCADA 시스템이라고 많이 불리는 이 시스템에서는 modbus, profibus 등의 단순한 프로토콜을 이용한다. 이 프로토콜들은 주로 근거리 통신을 위한 규격이지만, 
원격으로 데이터를 전송할 때는 TCP/IP 상에 데이터를 실어 보내는 방법들을 제공한다.

이상에서 우리가 흔히 알고 있는 컴퓨터 급의 장비들을 관리하기 위한 snmp와 
좀더 단순한 공장 자동화 장비들을 모니터링하고 제어하기 위한 modbus, 
profibus 프로토콜들이 사용되고 있는 것을 간단히 설명하였다. 본 글에서는 
범용 컴퓨터 급이 아닌 센서 등의 단순한 컴퓨터들에서 수집되는 정보들을 
어떻게 서버로 전달하고 이렇게 서버에 모아진 정보들을 운영자에게 보여줄 
것인지를 설명하고자 한다. 특히 최근에 화두가 되고 있는 big data, Internet
of Things, M2M 등에서도 이러한 내용들이 유용하게 활용될 수 있을 것으로
생각한다. 
또한 본 글에서는 ubuntu 리눅스를 기반으로 한 소프트웨어적인 측면만을 다룬다.


이 글에서 언급하고자 하는 원격 시스템이란 유선통신 연결이 매우 
어려운 지역에서 상용이동통신망을 이용하여 서버와 통신하는 장비를 
의미한다. 원격시스템은 스스로 센서를 가지고 있을 수도 있으며, 
근거리에서 시리얼 통신을 이용하여 데이터를 수집할 수 있다. 이렇게 
수집한 데이터를 서버로 전송하고 서버에서 들어오는 제어신호를 처리할 
수 있다. 원격 시스템은 ubuntu 리눅스를 OS로 사용하는 것을 가정한다.
