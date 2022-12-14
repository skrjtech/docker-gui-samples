# Docker GUI
GUIの表示に困っているピーポーに救済の手を...
## はじめに
リポジトリをクローンして移動
```
git clone https://github.com/skrjtech/docker-gui-samples.git
cd docker-gui-samples
```
イメージを作成
```
docker build -t pattern-gui ./pattern_a
```
## ホスト側にGUIを表示
ホスト側にアプリのウィンドウを開くには、以下のように設定していく \
xhostの許可
```
xhost +
```
out: access control disabled, clients can connect from any host \
許可後に下記のコマンドを実行
```
docker run --rm \
            -it \
            --name gui \
            -e DISPLAY=$DISPLAY \
            -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
            pattern-gui
```
コンテナの中に入ったらアプリを起動 \
```
xeyes
```
アプリが起動したら成功 \
Containerに, \
-e DISPLAY=$DISPLAY \
-v /tmp/.X11-unix:/tmp/.X11-unix:rw \
を与えることによってなんちゃらが共有されてGUIの表示ができる \
注意) サーバー/クライアントの場合では別な方法で起動する また後に紹介 \
docker compose にファイルを指定して起動
```
docker compose -f ./pattern_a/docker-compose.yml up -d
```
バックグラウンドで起動してアプリが起動したら成功
## ユーザーを追加して起動
引数を指定してイメージを作成
```
docker build -t pattern-gui:ver1 ./pattern_b    \
                    --build-arg USERNAME=main   \
                    --build-arg GROUPNAME=main  \
                    --build-arg PASSWORD=main   \
                    --build-arg UID=1000        \
                    --build-arg GID=1000
```
コンテナの起動
```
docker run --rm \
            -it \
            --name gui \
            -e DISPLAY=$DISPLAY \
            -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
            pattern-gui:ver1
```
コンテナの中に入ったらアプリを起動 \
```
xeyes
```
アプリが起動したら成功
docker compose にファイルを指定して \
単にビルド
```
docker compose -f ./pattern_b/docker-compose.yml build
```
ビルドしてから起動
```
docker compose -f ./pattern_b/docker-compose.yml up -d
```
バックグラウンドで起動してアプリが起動したら成功
## ホスト側のユーザーを追加してアプリを起動
```
docker run --rm \
            -it \
            --name gui \
            --user=$(id -u $USER):$(id -g $USER) \
            --env="DISPLAY" \
            --workdir="/home/$USER" \
            --volume="/home/$USER:/home/$USER" \
            --volume="/etc/group:/etc/group:ro" \
            --volume="/etc/passwd:/etc/passwd:ro" \
            --volume="/etc/shadow:/etc/shadow:ro" \
            --volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
            --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
            pattern-gui:latest
```
コンテナの中に入ったらアプリを起動 \
```
xeyes
```
アプリが起動したら成功
docker compose にファイルを指定して起動
```
docker compose -f ./pattern_c/docker-compose.yml up -d
```
バックグラウンドで起動してアプリが起動したら成功
## サーバーのアプリをクライアントで表示する
sshで接続
```
ssh -X username@192.168.1.2
```
x11-appsがサーバー側にインストールされていたら
```
xeyes
```
を実行して表示されていたら, とりあえず成功
サーバー側にリポジトリをクローン
```
git clone https://github.com/skrjtech/docker-gui-samples.git
cd docker-gui-samples
```
イメージを作成
```
docker build -t pattern-gui ./pattern_a
```
クライアント側からコンテナを起動
```
docker run --rm \
            -it \
            --name gui \
            -e DISPLAY=$DISPLAY \
            --mount type=bind,src=$HOME/.Xauthority,dst=/root/.Xauthority.copy \
            --net host \
            pattern-gui:latest \
            /bin/bash -c "cd ~; cp .Xauthority.copy .Xauthority; chown root:root .Xauthority; xeyes"
```
しばらく立つとアプリが起動される \
docker composeの場合 \
Containerを起動
```
docker compose -f pattern_d/docker-compose.yml up -d
```
しばらく立つとアプリが起動される  \
別パターンでユーザーを追加したパターン
```
docker compose -f pattern_e/docker-compose.yml up -d
```
## 最後にvscodeでGUI起動する際の話　あっ！サーバー・クライアント間での話しね！
まずクライアント側でvscodeを実行
```
code pattern_f/
```
〜ん、いろいろ試した結果何故かvscodeからコンテナを開くことができない...

できたところまで
直接起動
```
docker composer -f pattern_f/.docker/docker-compose.yml up -d
```
vscodeのRemote ExplorerからContainerに入る
あとはGUI表示準備をして
```
cp /home/main/.Xauthority.copy /home/main/.Xauthority
chown main:main /home/main/.Xauthority
```
最後にアプリを起動
```
xeyes
```
