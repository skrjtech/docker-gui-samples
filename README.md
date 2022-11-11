# Docker GUI
GUIの表示に困っているピーポーに救済の手を...
## はじめに
リポジトリをクローンして移動
```
git clone https://github.com/skrjtech/docker-ros-khi-robot.git
cd docker-ros-khi-robot
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
アプリが起動した成功
```
xeyes
```
docker compose にファイルを指定して起動
```
docker compose -f ./pattern_a/docker-compose.yml up -d
```
バックグラウンドで起動してアプリが起動した成功
## ユーザーを追加して起動
引数をしてしてイメージ作成
```
docker build -t pattern-gui:ver1 ./pattern_b    \
                    --build-arg USERNAME=main   \
                    --build-arg GROUPNAME=main  \
                    --build-arg PASSWORD=main   \
                    --build-arg UID=1000        \
                    --build-arg GID=1000
```