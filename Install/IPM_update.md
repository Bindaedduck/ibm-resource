## IBM Process Mining update
### [ Non Container ]
> Download
- [IPM - 2.1.1](https://partnerportal.ibm.com/s/software-access-catalog)
    - Part number: M10ZYML  
    - 다운 받은 파일 안에 ibmprocessmining-update-2.1.1_4496c91.tar.gz파일만 추출

> Steps
1. IPM v2.1.1 작업 폴더 설정
    ```
    # ibmprocessmining-update-2.1.1_4496c91.tar.gz 파일 opt폴더에 복사
    cp [ibmprocessmining-update-2.1.1_4496c91.tar.gz 파일 경로] /opt
    
    # 압축 풀기
    cd /opt
    sudo tar xvf ibmprocessmining-update-2.1.1_4496c91.tar.gz

    # PM_HOME 변수 설정
    export PM_HOME="/opt/processmining"
    ```

2. IPM v2.1.1 업데이트
    ```
    cd /opt/processmining-update

    # IPM 서비스 실행중이면 서비스 중지 후 실행
    ./update.sh
    ```

3. PostgreSQL update
    ```
    cd opt/processmining/utils

    ./postgres-utils.sh
    ```

4. 서비스 재시작 후 프로젝트 > 정보 > 버전 확인
