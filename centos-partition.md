---
title: MySQL 데이터베이스 서버용 Centos7 디스크 파티션
date: 2018-03-09 01:10:51
categories:
	- OS
	- linux
	- Centos7
tags: 
	- linux
	- DB
	- server
	- disk partition
---

{% img [Image of Centos] https://cdn-images-1.medium.com/max/1600/1*gflTiSSWljtfxCMm5i5m_Q.png %}

## 참고 사이트

- {% link How to install and configure a linux server for MySQL?  https://www.psce.com/en/blog/2012/04/12/how-to-install-and-configure-a-linux-server-for-mysql/%}

- {% link Best practices for configuring optimal MySQL Memory Usage https://www.percona.com/blog/2016/05/03/best-practices-for-configuring-optimal-mysql-memory-usage/ %}


## 사용할 데이터베이스 서버용 데스크 탑 사양

RAM : 4GB
HDD : 1TB
CPU : i5 (정확한 모델명은 확인해봐야겠다.)
Core : ?


## 파티션 방법

오로지 데이터베이스 서버를 위한 목적이면, 보통 4개의 파티션만 만들어줘도 충분하다.

**/boot**

250MB 또는 500MB 정도로 잡아주면 된다. 부트 이미지 파일의 용량이 그리 크지 않기

때문에 500MB 정도만 잡아도 5개 정도의 부트 이미지를 저장할 수 있다.

**/root** 

20GB 또는 30GB 정도 잡아준다. 여기엔 시스템 파일들과 로그들을 저장한다.

**swap**

4GB 정도 잡아주었다. MySQL은 시스템 스왑을 사용하지 않는다. 그래서 굳이 centos

intsall시 swap 파티션을 잡지 않으려 했는데, 안 잡으면 설치 진행이 안된다…

그래서 여기저기 조언을 구해 본 결과, 데이터베이스 서버 운영 중에 어떤 상황이 발생할 지

모르기 때문에 변수 통제를 위해 어느 정도 잡아주는 것이 좋다는 결론이 나왔다. 그래서

RAM이랑 동일한 크기로 일단 잡아보았다. 예외적인 상황을 위해 swap 파티션을 잡았지만,

동작 중에 지속적인 swapping이 발생하면 mySQL 서버의 퍼포먼스는 현저히 떨어지고 불필요한

메모리 사용이 발생한다. 

따라서 이를 막기 위해 /etc/sysctl.conf 에서 vm.swappiness = 0을 추가해준다.

**/mnt/db**

mySQL에 저장될 데이터와 binary log를 위해 나머지 용량을 모두 할당한다.
	

##파일 시스템

**ext3**나 **ext4**를 사용하면 된다.

하지만 만약 시스템 퍼포먼스의 마지막 한방울까지 짜낼 필요가 있는 상황이라면

**xfs**를 사용하면 된다. 왜냐하면 xfs가 concurrent I/O를 더 잘 처리할 수 있기 때문이다.

## 마치며
	
주변의 조언들과 구글링을 통해 정리해봤습니다. 하지만 특히 swap 파티션 부분은 저의 주관이

들어갔기 때문에 확실하지 않습니다. 이에 대해 수정할 부분이 있으면 댓글로 부탁드립니다.


