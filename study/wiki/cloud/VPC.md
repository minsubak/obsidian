(이 문서는 [AWS 강의 자료](https://www.youtube.com/watch?v=OGQ8RU-9oP8&list=PL-VaKu9hkhpcx7ElwUvKvYngdB86BAXOW&index=1)의 이미지를 인용하였습니다.)
(타 Cloud Platform과 용어의 차이가 존재하나 호환니다.)
# 정의
 Virtual Private Cloud
+ 사용자 정의 가상 네트워크 공간  

+ 완전한 네트워크 제어 가능
	- IP range
	- Subnet (public, private)
	- Route Table
	- Network-ACL, 보안 그룹
	- 다양한 gateway  

+ VPC 내 모든 EC2 instance는 사설IP 부여  

+ 개별 instance에 공인 IP 할당 가능
	- Public IP: 유동 IP
	- Elastic IP: 고정 IP  

| VPC_image1 |
| ---- |
| ![[VPC_image1.png]] |
# VPC와 클라우드 서비스 영역
| VPC_image2 |
| ---- |
| ![[VPC_image2.png]] |


# IP 주소 범위
+ CIDR 블록 설정
	+ Classless Inter-Domain Routing
	+ IP 대역을 Network Address + Host Address로 나누는 방식
	+ ex) 172.31.0.0/16
+ [[RFC 1918]]에 정의된 사설 IP 대역 사용 권고
+ VPC CIDR는 생성 후 변경 불가능
+ VPC의 네트워크 범위는 /16 ~ /28까지 가능
+ 추후 연결할 가능성이 존재하는 네트워크와 주소가 중복되지 않도록 할당하는 것을 권고
	+ On-Premises Netwok 연동 고려
	+ Cloud Platform 내 Region 간 확장  

# [[서브넷]]
+  VPC CIDR 블록 범위에서 Region 별 세부 서브넷 정의
	+ 서브넷 범위는 보통 /24 (256) 이상을 권고
	+ 보통 5개의 주소는 Cloud Platform에 의해 예약된 주소라 사용 불가
		+ NCP의 경우 1~5번째, AWS는 0~4번째, 마지막  
	
+ 서브넷 종류
	+ public: 인터넷 연결
	+ private: 외부망 차단
+ 서브넷 초기 생성 이후 주소 범위 변경 불가능  

| VPC_image3 |
| ---- |
| ![[VPC_image3.png]] |



#공부 #클라우드 #네트워크