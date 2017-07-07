# ER_TABLEACCESS_DENIED_ERROR: SELECT command denied to user

> 이 에러는 현재 접속을 시도하는 사용자에게 접속 시도 데이터베이스 접근 권한이 없는 경우
따라서, 접속하려는 데이터베이스 접근권한을 부여한다.

## 계정 확인
```sql
> use mysql;
> select host, user, password from user;
```

## 권한 확인
```sql
> show grants for user@localhost;
```

## 권한 부여
```sql
> grant all privileges on db_name.* to user@localhost identified by 'password';

> flush privileges;
```

- `all privileges`
    - CRUD 모든 권한을 부여

- grant **selet, update** on
    - C,U 권한만 부여

- `on db_name.*`
    - `on *.*`
        `.`을 기준으로 앞이 데이터베이스명 뒤가 테이블명
        `*`은 모든 데이터베이스, 테이블을 의미
        `on db_name.*` : 'db_name' 이라는 데이터베이스의 모든 테이블에 권한 부여
        `on db_name.users` : 'db_name' 이라는 데이터베이스의 'users' 테이블만 권한 부여

- flush privileges
    - 변경 사항 적용
