**AWS Database Migration Service 설정**

AWS Database Migration Service(AWS DMS)을 처음 사용하기 전에 다음 작업을 완료해야 합니다.

1. AWS에 가입
2. IAM 사용자 생성
3. AWS Database Migration Service의 마이그레이션 계획

**AWS에 가입**

Amazon Web Services(AWS)에 가입하면 AWS DMS를 포함해 AWS의 모든 서비스에 AWS 계정이 자동으로 등록됩니다. 사용한 서비스에 대해서만 청구됩니다.

AWS DMS를 사용하여 사용한 리소스에 대해서만 비용을 지불합니다. 생성한 AWS DMS 복제 인스턴스는 실시간으로 활성화됩니다(샌드박스에서 실행되지 않음). 종료하기 전까지 인스턴스에 대해 기본 AWS DMS 사용 요금이 청구됩니다. AWS DMS 사용 요금에 대한 자세한 내용은 [AWS DMS 제품 페이지](http://aws.amazon.com/dms)를 참조하십시오. AWS를 처음 사용하는 고객인 경우에는 AWS DMS를 무료로 시작할 수 있습니다. 자세한 내용은 AWS 프리 티어를 참조하십시오.

AWS 계정을 닫으면 2일 후에 계정과 연결된 모든 AWS DMS 리소스 및 구성이 삭제됩니다. 이 리소스에는 모든 복제 인스턴스, 원본 및 대상 엔드포인트 구성, 복제 작업, SSL 인증서가 포함됩니다. 2일이 지난 후에 AWS DMS를 다시 사용하기로 한 경우 필요한 리소스를 다시 생성해야 합니다.

이미 AWS 계정이 있다면 다음 작업으로 건너뛰십시오.

AWS 계정이 없는 경우 다음 절차에 따라 하나 만드십시오.

AWS에 가입하려면

1. https://aws.amazon.com/을 열고 [Create an AWS Account]를 선택합니다.
2. 온라인 지시 사항을 따릅니다.

다음 작업에 필요하므로 AWS 계정 번호를 기록합니다.

**IAM 사용자 생성**

AWS DMS와 같은 AWS 서비스에 액세스하려면 자격 증명을 제공해야 합니다. 리소스에 대한 액세스 권한이 있는지 여부를 파악해야 하기 때문입니다. 콘솔은 암호를 요구합니다. AWS 계정에 대한 액세스 키를 생성하면 명령줄 인터페이스 또는 API에 액세스할 수 있습니다. 그러나 AWS 계정에 자격 증명을 사용하여 AWS에 액세스하지 말고, AWS Identity and Access Management(IAM)를 사용하는 것이 좋습니다. IAM 사용자를 생성하여 관리자 권한과 함께 IAM 그룹에 추가하거나, 이 사용자에게 관리자 권한을 부여하십시오. 그러면 IAM 사용자의 특정 URL이나 자격 증명을 사용하여 AWS에 액세스할 수 있습니다.

AWS에 등록하였지만 자신의 IAM 사용자를 아직 생성하지 않았다면 IAM 콘솔에서 생성할 수 있습니다.

IAM 사용자를 직접 생성하여 Administrators 그룹에 추가하려면

1. https://console.aws.amazon.com/iam/에서 AWS 계정 루트 사용자 이메일 주소 및 암호를 사용하여 IAM 콘솔에 [AWS 계정 루트 사용자](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) 로그인합니다.

**참고**

관리자 IAM 사용자를 사용하는 아래 모범 사례를 준수하고, 루트 사용자 자격 증명을 안전하게 보관해 두실 것을 강력히 권장합니다. 몇 가지 [계정 및 서비스 관리 작업](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)을 수행하려면 반드시 루트 사용자로 로그인해야 합니다.

2. 콘솔의 탐색 창에서 [Users]를 선택한 다음 [Add user]를 선택합니다.

3. [User name]에 Administrator를 입력합니다.

4. AWS Management 콘솔 access 옆의 확인란을 선택하고 Custom password를 선택한 다음 텍스트 상자에 새 사용자의 암호를 입력합니다. 필요하다면 [Require password reset]을 선택하여 다음에 사용자가 로그인할 때 새 암호를 생성하도록 설정할 수 있습니다.

5. Next: Permissions(다음: 권한)를 선택합니다.

6. [Set permissions] 페이지에서 [Add user to group]을 선택합니다.

7. Create group을 선택합니다.

8. 그룹 생성 대화 상자의 그룹 이름에 Administrators를 입력합니다.

9. 필터 정책에서 AWS 관리 직무의 점검란을 선택합니다.

10. 정책 목록에서 AdministratorAccess 옆의 확인란을 선택합니다. 그런 다음 Create group을 선택합니다.

11. 그룹 목록으로 돌아가 새로 만든 그룹 옆의 확인란을 선택합니다. 목록에서 그룹을 확인하기 위해 필요한 경우 Refresh를 선택합니다.

12. Next: Review를 선택하여 새 사용자에 추가될 그룹 멤버십의 목록을 확인합니다. 계속 진행할 준비가 되었으면 Create user를 선택합니다.

이제 동일한 절차에 따라 그룹이나 사용자를 추가 생성하여 AWS 계정 리소스에 액세스할 수 있는 권한을 사용자에게 부여할 수 있게 되었습니다. 특정 AWS 리소스에 대한 사용자의 권한을 제한하는 정책을 사용하는 방법을 알아보려면 [액세스 관리](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/access.html) 및 [정책 예시](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/access_policies_examples.html) 단원으로 이동하십시오.

이 새로운 IAM 사용자로 로그인하려면 먼저 AWS 콘솔에서 로그아웃한 후 다음 URL을 사용합니다. 여기에서 `your_aws_account_id`는 하이픈을 제외한 AWS 계정 번호를 나타냅니다. 예를 들어, AWS 계정 번호가 1234-5678-9012이면 계정 ID는 123456789012입니다.

```
https://your_aws_account_id.signin.aws.amazon.com/console/
```

방금 생성한 IAM 사용자 이름과 암호를 입력합니다. 로그인하면 탐색 모음에 "your_user_name @ your_aws_account_id"가 표시됩니다.

*로그인 페이지의 URL에 AWS 계정 ID가 포함되지 않게 하려면 계정 별칭을 생성합니다. IAM 대시보드에서 [Customize]를 선택하고 회사 이름 등의 별칭을 입력합니다. 계정 별칭 생성 후 로그인할 때는 다음 URL을 사용합니다.*

```
https://your_account_alias.signin.aws.amazon.com/console/
```
본인 계정의 IAM 사용자 로그인 링크를 확인하려면 IAM 콘솔을 열고 대시보드의 [AWS Account Alias] 아래를 체크합니다.

**AWS Database Migration Service의 마이그레이션 계획**

AWS Database Migration Service를 사용하여 데이터베이스 마이그레이션을 계획할 때에는 다음 사항을 고려해야 합니다.

- 원본과 대상 데이터베이스를 AWS DMS 복제 인스턴스에 연결하는 네트워크를 구성해야 합니다. 이 작업은 VPN을 통해 온프레미스 데이터베이스를 Amazon RDS DB 인스턴스에 연결하는 것과 같이 복제 인스턴스와 동일한 VPC에서 AWS 리소스 2개를 보다 복잡한 구성에 연결하는 것만큼이나 간단합니다. 자세한 내용은 [데이터 마이그레이션을 위한 네트워크 구성](https://docs.aws.amazon.com/ko_kr/dms/latest/userguide/CHAP_ReplicationInstance.VPC.html#CHAP_ReplicationInstance.VPC.Configurations)을(를) 참조하십시오.

- 원본 및 대상 엔드포인트 – 대상 데이터베이스로 마이그레이션해야 하는 원본 데이터베이스의 정보와 테이블을 알고 있어야 합니다. AWS DMS는 테이블 및 기본 키 생성을 포함하여 기본 스키마 마이그레이션을 지원합니다. 그러나, AWS DMS는 보조 인덱스, 외부 키, 저장 프로시저, 사용자 계정 등은 대상 데이터베이스에 자동으로 생성하지 않습니다. 원본 및 대상 데이터베이스 엔진에 따라 보충 로깅을 설정하거나 원본 또는 대상 데이터베이스의 다른 설정을 수정해야 할 수도 있습니다. 자세한 내용은 데이터 마이그레이션용 원본 및 대상 마이그레이션에 적합한 대상 섹션을 참조하십시오.

스키마/코드 마이그레이션 – AWS DMS에서는 스키마나 코드 변환을 수행하지 않습니다. Oracle SQL Developer, MySQL Workbench 또는 pgAdmin III와 같은 도구를 사용하여 스키마를 변환할 수 있습니다. 기존 스키마를 다른 데이터베이스 엔진으로 변환할 경우, AWS Schema Conversion Tool을 사용할 수 있습니다. 대상 스키마를 생성하고 전체 스키마(테이블, 인덱스, 보기 등)을 생성할 수도 있습니다. 또한 PL/SQL 또는 TSQL을 PgSQL 또는 다른 형식으로 변환하는 도구를 사용할 수도 있습니다. AWS Schema Conversion Tool에 대한 자세한 내용은 AWS Schema Conversion Tool 을 참조하십시오.

지원되지 않는 데이터 형식 – 일부 원본 데이터 형식을 대상 데이터베이스의 해당 데이터 형식으로 변환해야 합니다. 지원되는 데이터 형식에 대한 자세한 내용은 데이터 스토어의 원본 또는 대상 섹션을 참조하십시오.
