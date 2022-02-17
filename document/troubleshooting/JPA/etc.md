[transaction 영역에서 error 확인하는 방법]
<br> e.printStackTrace() 추가
```
catch (Exception e) {
  transaction.rollback();
  e.printStackTrace();
}
```
예) 아래처럼 에러 원인을 볼 수 있다 <br>
org.hibernate.QueryException: could not resolve property: name of: jpal.Member [select m From jpal.Member m where m.name like 'member%']
