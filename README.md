# 🐳 Docker Image 최적화 핵심 전략
<br/>

## 🚪 들어가기 전
도커 이미지 최적화는 다양한 이점을 제공하며, 그 중 가장 중요한 것은 자원 효율성을 높일 수 있다는 점입니다. 작은 이미지는 컨테이너가 사용하는 메모리와 CPU 자원을 줄여, 동일한 하드웨어에서 더 많은 컨테이너를 실행할 수 있게 합니다.

이번에는 도커 이미지 최적화 방법을 심층적으로 연구하고 탐구를 진행해보겠습니다. 최적화를 진행하지 않으면 불필요한 리소스 소모와 성능 저하가 발생할 수 있으며, 이미지 크기가 크면 다운로드 및 배포 시간이 길어져 서버의 스케일 아웃 시간에 영향을 미칠 수 있습니다.

이 과정을 통해 제 기술과 지식을 더욱 발전시키고자 합니다.
<br/><br/>

## 💻 시스템 환경 및 소프트웨어
**Ubuntu 22.04.5 LTS** <br/>
**Docker version 27.3.1** <br/>
**Visual studio code** <br/>
<br/><br/>

## 🔎 사전 작업
### Dockerfile 파일 위치 할 폴더 생성
```bash
mkdir step03Image
```

### Dockerfile 파일 위치
```bash
username@servername:~/step03Image$ tree
.
└── Dockerfile
```
<br/><br/>

## 🔧 Docker Image 최적화 방법 1 : 최소한의 기본 이미지를 사용

### Ubuntu 기반의 Nginx 이미지 
**1. Dockerfile 작성**
```bash
# 베이스 이미지 설정
FROM nginx

COPY ./config/nginx.conf /etc/nginx/conf.d/nginx.conf
COPY ./html/index.html /usr/share/nginx/html/index.html

# 컨테이너 실행시 명령어: Nginx를 포그라운드에서 실행
CMD ["nginx", "-g", "daemon off;"]
```
**2. Docker 이미지 빌드**
```bash
docker build -t ubuntunginx .
```
<br/>

### Alpine Linux를 기반으로 한 Nginx 이미지
- Alpine Linux는 경량화된 Linux 배포판으로, 주로 보안과 효율성을 중시하여 설계됨 <br/>

**1. Dockerfile 작성**
```bash
# 베이스 이미지 설정
FROM nginx:alpine

COPY ./config/nginx.conf /etc/nginx/conf.d/nginx.conf
COPY ./html/index.html /usr/share/nginx/html/index.html

# 컨테이너 실행시 명령어: Nginx를 포그라운드에서 실행
CMD ["nginx", "-g", "daemon off;"]
```
**2. Docker 이미지 빌드**
```bash
docker build -t alpinenginx .
```

<br/>

### 결과:두 이미지 용량 비교
 - alpinenginx가 ubuntunginx보다 144.8 MB 더 작음.
 - 상대적으로, alpinenginx는 ubuntunginx의 약 **23%** 에 해당하는 용량.
```bash
username@servername:~$ docker images
REPOSITORY              TAG       IMAGE ID       CREATED         SIZE
alpinenginx             latest    c7b4f26a7d93   5 weeks ago     43.2MB
ubuntunginx             latest    39286ab8a5e1   5 weeks ago     188MB
```
**→ alpinenginx 이미지가 더 가벼운 선택이며, 용량이 적은 이미지가 필요할 때 유용**

<br/>

## 🧪 References
### 참고1
[Reduce the Size of the Docker Image](https://faun.pub/reduce-the-size-of-the-docker-image-e6895b653419)

### 참고2
[Docker Image Optimization Tips & Tricks](https://overcast.blog/docker-image-optimization-tips-tricks-6a17f687162b)


