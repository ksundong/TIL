# 브라우저에서 서버 통신과정

## 개요

보통 질문은 주소창에 url을 입력했을 때, 어떻게 통신이 이루어지는지 질문을 합니다.  
https의 경우 추가적인 통신과정이 필요한데, 대개의 신입면접에서는 그정도까지 요구하지는 않는 편입니다. 그래도 간단히 다뤄보고자 합니다.

먼저 보통 우리가 입력하는 url은 protocol, domain, subdomain, path 정도로 나눌 수 있는데 여기서 중요한 부분은 domain입니다.

이 domain을 가지고는 브라우저가 서버에 요청을 보낼 수 없습니다. domain에 대해서 안다면, domain은 사람이 ip를 외우기 어렵기 때문에 외우기 쉬운 문자형식으로 ip와 매핑을 합니다. 흔히 DNS Table이라고 부르죠. 즉, ip를 통해서 요청을 보내야 합니다.

## DNS Lookup

따라서 ip를 알아내는 과정이 필요합니다. 이를 DNS Lookup 이라고 합니다. DNS 서버의 통신은 **UDP통신**을 기반으로 합니다.

[브라우저에서 DNS Lookup은 어떤과정으로 진행되는가](../Network/browser-dnslookup-process.md)

이 자료는 제가 Chrome 브라우저와 MacOS 환경에서 테스트한 내용을 기반으로 작성되어 있습니다.

## TCP 3Way Handshake

이렇게 ip를 알아냈다면 HTTP 통신은 TCP를 이용하므로 TCP 3way handshaking 을 통해 데이터를 전송할 준비가 됬음을 알아냅니다. TCP 3way handshake는 TCP를 사용하는 장치들 사이에 논리적인 접속을 정립하기 위해 사용됩니다.

TCP 프로토콜은 정확한 전송을 보장하는 것을 목적으로 하므로 연결상태를 확인하는 과정을 TCP 3way handshake라고 합니다.

간단하게 정리하자면

1. Client측에서 Server 측에 SYN(n) 패킷을 전송합니다.
2. Server 측에서는 SYN(m)과 ACK(n+1) 패킷을 전송합니다. (한 패킷에 담겨서 전송됩니다.)
3. Client 측에서는 다시 ACK(m+1) 패킷을 전송합니다.

위의 과정중에서 오류가 발생한다면 연결이 불안정함을 알 수 있습니다.

위의 과정을 거치고 나면 TCP 소켓이 형성됩니다.

## HTTPS에서만 추가되는 SSL(TLS) Handshake

SSL의 마지막 버전 3.0은 2015년 이후로 지원 중단되었습니다. 대신 TLS로 대체되었습니다. 하지만 대부분 SSL이라고 부르므로 둘 다 사용한다고 생각하시면 됩니다.

HTTP 2.0은 HTTPS에서만 지원됩니다.

TLS에서는 두 가지 암호화 방식을 사용하는데, 비대칭키 암호화와 대칭키 암호화를 사용합니다.

간단하게 설명하자면, 비대칭키 암호화는 비용이 많이 들기 때문에 실제 메시지를 전송하는 구간에서는 대칭키 암호화를 사용합니다. 비대칭키 암호화는 상대방이 내가 통신하려는 상대가 맞는지 확인하는데 이용됩니다.

1. Client는 ACK를 보내고 나서 Client Hello를 전송합니다. TLS 버전과, 지원하는 암호화 모음을 적어서 서버에게 전송합니다.
2. Server는 Chiper Suite 목록중 자신이 지원하는 것을 적어서 보냅니다. 공통된 방식이 없는 경우 연결은 끊어집니다. 그리고 인증서도 보냅니다.
3. Client는 서버의 인증서를 평가합니다. 그리고 ClientKeyExchange에 세션 키를 만들어서 서버의 공개키로 암호화해서 전달합니다.
4. ChangeCipherSpec은 이후 메시지는 암호화됨을 알리는 용도입니다.(Server, Client 모두)
5. 이후 통신과정은 세션키로 암호화되는 공개키 암호화로 암호화되어 통신됩니다.

TLS 과정은 일반 통신보다 지연이 있지만 HTTP 2.0 스펙을 지원하기 때문에 실질적으로 더 빠릅니다.

인증서를 인증하는 과정

1. 서버는 정보와 공개키를 인증기관(CA)에 제공합니다.
2. CA는 검증을 거치고 CA의 개인키로 암호화된 인증서를 발급합니다.
3. 클라이언트가 접속요청을 하면 서버는 인증서를 전달합니다.
4. 인증서를 받은 클라이언트는 브라우저에 내장된 CA의 공개키로 인증서를 검증합니다.
5. 사이트 정보와, 서버의 공개키를 얻습니다.
6. 서버의 공개키로 대칭키를 암호화해서 서버에 전송합니다.

이 과정이 성립하려면 CA가 확실해야하고, 서버측에서 제공한 인증서 또한 확실해야 하며, 클라이언트가 대칭키를 암호화 할 때 암호화하는 공개키도 확실해야합니다. 이런 복잡한 과정으로 신뢰를 얻습니다.

## HTTP 통신 과정

브라우저는 자체적으로 네트워크를 통해 요청을 보낼 수 없습니다. OS에 Request 송신을 요청해야 합니다. 이를 담당하는 부분을 프로토콜 스택이라고 합니다.

Request 메시지는 프로토콜 스택에서 패킷에 담고 수신처의 주소 등을 제어 정보에 담아 LAN 어댑터에 의해 전기 신호로 변환되어 패킷이 네트워크 속으로 들어가게 됩니다.

이 패킷들은 스위칭 허브 등을 경유하여 최종적으로 인터넷 접속용 라우터에 도착합니다. 이후 통신사(provider)가 패킷을 운반합니다.

provider가 운반한 패킷은 웹 서버측의 LAN에 도착하면 방화벽에서 패킷을 검사합니다. 그리고 캐시 서버가 있다면 다시 이용할 수 있는 데이터는 캐시 서버에서 응답하게 됩니다.

또한 대규모 웹 사이트라면 로드 밸런서와 같은 부하 분산 장치가 설치되어 있을 수 있습니다.

웹 서버에 도착한 패킷은 웹 서버의 프로토콜 스택에 의해 원래 리퀘스트 메시지로 복원되고, 이를 웹 애플리케이션 서버에 전달해줍니다.

웹 애플리케이션 서버는 리퀘스트 메시지를 읽고 이에 적당한 응답을 작성하여 클라이언트에게 다시 보냅니다.

이 과정은 메시지 송신과 정확히 반대로 동작하게 됩니다.

응답이 클라이언트에 도착하면 데이터를 추출하여 화면에 표시하게 됩니다.

## TCP 4Way Handshake

TCP 통신을 종료하기 위한 작업입니다.

1. Client에서 종료를 알리는 FIN 패킷을 전송합니다.
2. Server에서 FIN을 받았다는 ACK 패킷을 전송하고, CLOSE-WAIT 상태가 됩니다.
3. 연결을 종료한 후 Server가 Client에 FIN 패킷을 전송합니다.
4. Client는 FIN을 받았다는 ACK 패킷을 전송하고, TIME-WAIT 상태가 됩니다.
5. Server는 소켓을 Close합니다.
6. TIME-WAIT 상태에서는 FIN을 수신해도, 일정시간 접속이 유지되며 도착하지 않은 패킷을 기다립니다.

## References

성공과 실패를 결정하는 1%의 네트워크 원리

<https://real-dongsoo7.tistory.com/73>

<https://wayhome25.github.io/cs/2018/03/11/ssl-https/>

<https://aws-hyoh.tistory.com/entry/HTTPS-%ED%86%B5%EC%8B%A0%EA%B3%BC%EC%A0%95-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-2Key%EA%B0%80-%EC%9E%88%EC%96%B4%EC%95%BC-%EB%AC%B8%EC%9D%84-%EC%97%B4-%EC%88%98-%EC%9E%88%EB%8B%A4?category=768734>

<https://unit42.paloaltonetworks.com/wireshark-tutorial-decrypting-https-traffic/>
