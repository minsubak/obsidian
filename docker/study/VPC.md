(이 문서는 Naver Cloud Platform의 VPC 설명을 인용합니다. 
VPC 매뉴얼 : (https://guide.ncloud-docs.com/docs/networking-vpc-vpcoverview))

VPC(Virtual Private Cloud)는 퍼블릭 클라우드 환경에서 사용할 수 있는 고객 전용 사설 네트워크입니다. 다른 네트워크와 논리적으로 분리되어있어 IT 인프라를 안전하게 구축하고 간편하게 관리할 수 있습니다. 또 기존의 데이터 센터 네트워크와 유사하게 구현할 수 있습니다.

### VPC가 제공하는 기능
+ 전용 네트워크: 타 네트워크와 간섭이 발생할 염려 없이 논리적으로 완전히 분리된 네트워크를 사용할 수 있습니다.
+ 다양한 네트워크 토폴로지: VPC 내부에 Public Subnet 또는 Private Subnet을 생성하여 고객 맞춤형 네트워크 환경을 조성할 수 있습니다.
+ 강력한 보안: ACG(Access Control Group)와 NACL(Network Access Control List)를 통해 네트워크의 접근을 제어합니다. ACG는 서버 단계의 접근을 제어하며, NACL은 Subnet 단계의 접근을 제어합니다.
+ 외부 네트워크와의 보안 통신: VPC와 고객 사이트 간 안전한 통신을 위해 Cloud Connect와 Managed IPsec VPN을 사용할 수 있습니다.
+ VPC간 내부 통신: VPC Peering을 사용해 다른 VPC와 통신할 수 있습니다. 공인 IP 없이 내부 네트워크로 통신하기 때문에 비용의 효율성이 높아집니다.
