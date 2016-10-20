#Ubuntu Trusty 14.04 (LTS) 安装docker

  https://github.com/yeasy/docker_practice/blob/master/install/ubuntu.md

  sudo docker pull ubuntu:14.04

#创建ubuntu-golang镜像 /home/ubuntu/Dockerfile
  
  FROM ubuntu:14.04

  RUN apt-get update

  RUN apt-get install curl -y
  
  sudo docker build -t="test/ubuntu" .

#创建golang镜像 /home/golang/Dockerfile

  FROM test/ubuntu

  ENV GOLANG_VERSION 1.7.3
  
  ENV GOLANG_DOWNLOAD_URL https://golang.org/dl/go$GOLANG_VERSION.linux-amd64.tar.gz
  
  ENV GOLANG_DOWNLOAD_SHA256 508028aac0654e993564b6e2014bf2d4a9751e3b286661b0b0040046cf18028e

  RUN curl -fsSL "$GOLANG_DOWNLOAD_URL" -o golang.tar.gz \
  
          && echo "$GOLANG_DOWNLOAD_SHA256 golang.tar.gz" | sha256sum -c - \
		  
          && tar -C /usr/local -xzf golang.tar.gz \
		  
          && rm golang.tar.gz

  ENV GOPATH /go
  
  ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH
  
  RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" "$GOPATH/pkg" && chmod -R 777 "$GOPATH"

  WORKDIR $GOPATH
  
  sudo docker build -t="test/golang" .


#创建golang项目 /home/Go/src

  以及在src下创建Dockerfile文件
  
  FROM test/golang

  ADD . /go/src/app

  RUN go install -v app

  CMD ["app"]
  
  sudo docker build -t="testapp" .

#运行容器

  sudo docker run -it --rm --name test testapp

详情请参考官网


