## 아래 URL 에 큐브리드 pull
## https://hub.docker.com/r/cubrid/cubrid/tags?utm_source=chatgpt.com

# CentOS 7 기반
FROM centos:7

# 기본 그룹/유저 생성
RUN groupadd -r cubrid && useradd -r -g cubrid -d /home/cubrid -m cubrid

# /tmp 권한 변경
RUN chmod 777 -R /tmp

# yum 미러 변경
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-* \
    && sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-* 

# gosu 설치
ENV GOSU_VERSION 1.16
RUN yum -y install epel-release wget glibc-langpack-ko \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true \
    && yum clean all

# 환경 변수: Cubrid
ENV CUBRID_VERSION 11.2-latest
ENV CUBRID /home/cubrid/CUBRID
ENV CUBRID_DATABASES /var/lib/cubrid
ENV CUBRID_DB cubdb
ENV CUBRID_VOLUME_SIZE 100M
ENV CUBRID_LOCALE ko_KR.utf8
ENV CUBRID_COMPONENTS ALL
ENV PATH $CUBRID/bin:$PATH
ENV LD_LIBRARY_PATH=$CUBRID/lib

# Cubrid 설치
RUN wget -O cubrid.tar.gz http://ftp.cubrid.org/CUBRID_Engine/11.2_latest/CUBRID-$CUBRID_VERSION-Linux.x86_64.tar.gz \
    && tar -xzf cubrid.tar.gz -C /home/cubrid \
    && rm cubrid.tar.gz \
    && mkdir -p $CUBRID_DATABASES \
    && chown -R cubrid:cubrid $CUBRID $CUBRID_DATABASES

# 데이터베이스 볼륨
VOLUME $CUBRID_DATABASES

# 엔트리포인트
COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# 포트 40000 노출
EXPOSE 40000



