# jenkins 学習のためのリポジトリ

WSL2 上で行う

## 環境構築

https://www.jenkins.io/doc/book/installing/docker/#setup-wizard に従って行う

以下のコマンドを実行

```sh
docker network create jenkins

docker run --name jenkins-docker --rm --detach --privileged --network jenkins --network-alias docker --env DOCKER_TLS_CERTDIR=/certs --volume jenkins-docker-certs:/certs/client --volume jenkins-data:/var/jenkins_home --publish 2376:2376 docker:dind --storage-driver overlay2

docker build -t myjenkins-blueocean:2.319.3-1 .

docker run --name jenkins-blueocean --rm --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --publish 8080:8080 --publish 50000:50000 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro myjenkins-blueocean:2.319.3-1
```

`http://localhost:8080`にアクセス。以下の表示がされれば成功

![](./images/jenkins_start.jpg)

password を求められるので表示に書いてあるファイルを読んでフォームに入力

```sh
docker container exec -it jenkins-blueocean bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

Install suggested plugins を選択して、ユーザー名やパスワードなどを設定して構築完了
ローカル環境ではユーザー名：developer、パスワード：developer とした。

![](./images/jenkins_home.jpg)

## 最初のジョブ
