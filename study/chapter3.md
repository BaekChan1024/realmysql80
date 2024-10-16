# 사용자 및 권한

#### Quiz

<details>
  <summary>MySQL에서 사용자 계정을 생성할 때, 특정 IP 주소나 호스트 이름을 지정하지 않고 모든 외부 컴퓨터에서 접속 가능한 계정을 만들고 싶다면, 호스트 부분을 어떻게 지정해야 하나요?</summary>
  <p>호스트 부분을 <code>%</code>로 지정해야 합니다. <code>%</code>는 모든 IP 주소 또는 모든 호스트 이름을 의미하며, 이는 외부 컴퓨터 어디에서든 해당 계정으로 접속할 수 있게 해줍니다.</p>
</details>

<details>
  <summary>다음과 같은 두 계정이 MySQL 서버에 존재한다고 가정합니다: 
  <code>'svc_id'@'192.168.0.10'</code> 와 <code>'svc_id'@'%'</code>. 
  IP 주소가 192.168.0.10인 PC에서 svc_id 계정으로 접속을 시도할 때, MySQL은 어떤 계정을 먼저 선택하고, 그 이유는 무엇인가요?</summary>
  <p>MySQL은 <code>'svc_id'@'192.168.0.10'</code> 계정을 먼저 선택합니다. 이유는 MySQL이 더 구체적인 호스트(이 경우, 특정 IP 주소)로 정의된 계정을 우선적으로 선택하기 때문입니다. <code>%</code>는 모든 IP를 의미하지만, <code>192.168.0.10</code>은 특정 IP로 더 좁은 범위를 지정하기 때문에 우선 선택됩니다.</p>
</details>

<details>
  <summary>MySQL에서 동일한 사용자 아이디와 비밀번호로 다른 호스트에서 접속할 수 있게 설정하려면, 어떤 요소를 주의해야 하나요?</summary>
  <p>동일한 사용자 이름과 비밀번호로 다른 호스트에서 접속할 수 있게 하려면, 사용자 계정의 호스트 부분을 정확하게 설정해야 합니다. 예를 들어, IP 주소를 명확히 지정하거나, <code>%</code>를 사용하여 모든 호스트에서 접속 가능하게 설정해야 합니다. 동일한 사용자 이름이 다른 호스트에 대해 각각 다른 계정으로 등록되어 있으면, 더 구체적인 계정이 먼저 선택될 수 있으니 주의해야 합니다.</p>
</details>

<details>
  <summary>IP 주소가 192.168.0.10인 사용자가 'svc_id'라는 사용자로 MySQL 서버에 접속하려고 합니다. 이때, 해당 사용자가 비밀번호를 잘못 입력하면 어떤 결과가 발생할 수 있으며, 어떤 계정과 관련된 인증 문제일 가능성이 있나요?</summary>
  <p>만약 사용자가 잘못된 비밀번호를 입력하면, MySQL은 로그인 시도를 거절합니다. 이 경우 <code>'svc_id'@'192.168.0.10'</code> 계정이 먼저 선택되었지만, 해당 계정의 비밀번호가 일치하지 않으면 인증이 실패합니다. <code>'svc_id'@'%'</code> 계정도 존재하지만, MySQL은 더 구체적인 계정(<code>'svc_id'@'192.168.0.10'</code>)을 우선적으로 선택하므로 그 계정의 인증을 시도하게 됩니다.</p>
</details>

## 사용자 식별

### 시스템 계정과 일반 계정

#### Quiz

<details> <summary>MySQL 서버에서 'mysql.sys'@'localhost' 계정의 주요 역할은 무엇입니까?</summary> <p>'mysql.sys'@'localhost' 계정은 MySQL 8.0부터 기본으로 내장된 sys 스키마의 객체(뷰, 함수, 프로시저)들의 DEFINER로 사용됩니다. 이 계정은 서버 관리 및 모니터링에 필요한 시스템 객체들의 권한을 설정하는 데 사용됩니다.</p> </details>
<details> <summary>MySQL에서 계정(Account)과 사용자(User)의 차이점은 무엇인가요?</summary> <p>MySQL에서 "계정"은 서버에 로그인하기 위한 식별자를 의미하고, "사용자"는 서버를 사용하는 주체(사람 또는 응용 프로그램)를 의미합니다. 즉, 사용자는 MySQL 서버와 상호작용하는 주체이고, 계정은 그 주체가 로그인하기 위해 필요한 자격 정보입니다.</p> </details>
<details> <summary>'mysql.session'@'localhost' 계정은 어떤 상황에서 사용되며, 왜 처음부터 잠겨 있을까요?</summary> <p>'mysql.session'@'localhost' 계정은 MySQL 클라이언트가 서버에 접근할 때 사용되는 계정입니다. 이 계정은 서버가 자동으로 세션을 관리하기 위해 사용되며, 처음부터 잠겨 있는 이유는 악의적인 사용을 방지하기 위해서입니다.</p> </details>
<details> <summary>MySQL에서 내장된 계정들을 삭제하지 말아야 하는 이유는 무엇인가요?</summary> <p>내장된 계정들은 MySQL 시스템의 중요한 기능을 수행하는 데 필수적입니다. 예를 들어, sys 스키마나 session 관리 등에 사용되기 때문에 이러한 계정을 삭제하면 서버의 정상적인 동작에 문제가 발생할 수 있습니다. 또한, 이 계정들은 보안을 위해 잠겨 있어서 일반적인 접근이 불가능합니다.</p> </details>
<details> <summary>다음 SQL 쿼리를 통해 확인할 수 있는 3개의 내장된 MySQL 계정의 역할을 설명하세요:</summary> <p>```sql<br>SELECT user, host, account_locked FROM mysql.user WHERE user LIKE 'mysql.%';<br>```<br>1. 'mysql.sys'@'localhost': sys 스키마의 객체들의 DEFINER로 사용되는 계정입니다.<br>2. 'mysql.session'@'localhost': MySQL 클라이언트가 서버에 접근할 때 사용되는 계정입니다.<br>3. 'mysql.infoschema'@'localhost': information_schema에 정의된 뷰의 DEFINER로 사용되는 계정입니다. 이 계정들은 모두 처음부터 잠겨 있어서 악의적인 사용이 방지됩니다.</p> </details>

## 사용자 계정 관리

## 비밀번호 관리

## 권한

## 역할