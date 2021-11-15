## 2.3 샘플데이터베이스 설치 
- employees.zip 다운로드 
- 적당한 디렉토리에서 압축해제 
- 압축을 해제한 디렉토리에서 MYSQL 접속

```
# mysql u -root -p 
```
- Mysql에 접속되면 다음의 sql문 실행 
```
# source employees sql; 
```
- 실행이 끝나면 데이터베이스를 확인해보자
```
# show databases; 
```
- employees 스키마가 생성된 것을 확인할 수 있다. 
