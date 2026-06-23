
### 설치 파일 목록
| 파일명                                        | 설명                    | 비고       |
| ------------------------------------------ | --------------------- | -------- |
| `9.0.5-WS-WAS-FP020.zip`                   | WAS FP 9.0.5.20       | Fix Pack |
| `9.0.5.20-WS-WAS-IFPH71453.zip`            | WAS IFPH71453         | 임시패치     |
| `ibm-java-sdk-8.0-6.26-all-installmgr.zip` | IBM Java SDK 8 IM 저장소 | option   |


---

### 1. 설치 디렉토리 준비
```bash
# FP 9.0.5.20 디렉토리
mkdir -p /../install/fp20

# IFPH 71453 디렉토리
mkdir -p /../install/ifph71453

#Java SDK 디렉토리(option)
mkdir -p /../install/java-sdk
```

---

### 2. 파일 압축 풀기
```bash
# FP 9.0.5.20
unzip 9.0.5-WS-WAS-FP020.zip -d /../install/fp20

# IFPH 71453
unzip 9.0.5.20-WS-WAS-IFPH71453.zip -d /../install/ifph71453

# Java SDK 저장소(option)
unzip ibm-java-sdk-8.0-6.26-all-installmgr.zip -d /../install/java-sdk
```

---

### 3. Java SDK 8 설치(Option)
```bash
/../InstallationManager/eclipse/tools/imcl install \
  com.ibm.java.jdk.v8 \
  -repositories /../install/java-sdk \
  -installationDirectory /../WebSphere/AppServer \
  -acceptLicense
```

> **참고 - Offering ID**
> - Java SDK 8: `com.ibm.java.jdk.v8`
> - WAS ND: `com.ibm.websphere.ND.v90`

---

### 4. FP 27 업데이트
```bash
# 서버 중지
/../WebSphere/AppServer/profiles/<프로필이름>/bin/stopServer.sh <서버인스턴스이름> -username <유저명> -password <비밀번호>

# FP 20 업데이트
/../InstallationManager/eclipse/tools/imcl install \
  com.ibm.websphere.ND.v90 \
  -repositories /../install/fp20 \
  -installationDirectory /../WebSphere/AppServer \
  -acceptLicense

# 버전 확인
/../WebSphere/AppServer/bin/versionInfo.sh
```

---

### 5. IFPH71453 패치
```bash
# IFPH71453 적용
/../InstallationManager/eclipse/tools/imcl install \
  9.0.5.20-WS-WAS-IFPH71453 \
  -repositories /../install/ifph71453 \
  -installationDirectory /../WebSphere/AppServer \
  -acceptLicense

# 버전 확인
/../WebSphere/AppServer/bin/versionInfo.sh -ifixDetail

# 서버 재시작
/../WebSphere/AppServer/profiles/<프로필이름>/bin/startServer.sh <서버인스턴스이름>
```

--- 

### 6. 참고 사항
- IFPH71453 패치는 WAS 9.0.5.20 버전부터 적용 가능합니다.
- WAS 9.0.5.29 버전부터는 IFPH71453 패치가 적용된 채로 출시됩니다.
  - https://www.ibm.com/support/pages/node/7274233
  
- 접속 주소: https://localhost:9043/ibm/console

- Rollback
  - 단, Rollback이 가능하려면 IM이 이전 버전 파일을 **shared resources 디렉토리**에 보관하고 있어야 합니다. 
  - 기본적으로 이전 버전을 기억하고 있습니다.
```bash
# 설치된 버전 이력 확인
/../InstallationManager/eclipse/tools/imcl listInstalledPackages -long

# 롤백 (이전 버전으로)
/../InstallationManager/eclipse/tools/imcl rollback \
com.ibm.websphere.ND.v90 \
-installationDirectory /../WebSphere/AppServer
```