# samba container

|名称|値|備考|
|:-:|:-:|:-:|
|コンテナ名|samba||
|ボリューム名|samba|samba.volumeで指定|
|netbios port|13137-13138/udp|sambaコンテナ内で指定|
|samba port|44445/tcp, 13139/tcp|sambaコンテナ内で指定|

## 実行スクリプト

### 共通

```sh
cd Path/to/podman-container-samba
sudo firewall-cmd --permanent --new-service=user-samba
sudo firewall-cmd --permanent --service=user-samba --add-port=44445/tcp
sudo firewall-cmd --permanent --service=user-samba --add-port=13139/tcp
sudo firewall-cmd --permanent --service=user-samba --add-port=13137-13138/udp
sudo firewall-cmd --permanent --add-service=user-samba

sudo firewall-cmd --reload
```

### Quadlet使用時

```sh
mkdir -p $HOME/.config/containers/systemd/
cp Quadlet/* $HOME/.config/containers/systemd/
systemctl --user daemon-reload

systemctl --user start podman_build_samba
systemctl --user start podman_container_samba
```

### Quadlet非使用時

```bash
podman build --tag samba --file samba/Dockerfile .
podman run --name samba --publish 13137-13138:13137-13138/udp --publish 13139:13139/tcp --publish 44445:44445/tcp --mount type=volume,source=samba,destination=/V --detach --replace samba
```
