# Netdata初步探索
## Netdata簡介

Netdata 是一款 Linux 性能實時監測工具。以web的可視化方式展示系統及應用程序的實時運行狀態（包括cpu、內存、硬盤輸入/輸出、網絡等linux性能的數據）。是很像 Nagios 之類的監控軟體，但僅能借由通過 Web 介面進行即時監控。它的 web 前端響應很快，而且不需要 Flash 插件。 UI 很整潔，保持著 Netdata 應有的特性。第一眼看上去，你能夠看到很多圖表，幸運的是絕大多數常用的圖表數據（像 CPU，RAM，網絡和硬碟）都在頂部。如果你想深入了解圖形化數據，你只需要下滑滾動條，或者點擊在右邊菜單的項目。通過每個圖表的右下方的按鈕， Netdata 還能讓你控制圖表的顯示，重置，縮放。目前 Netdata 還沒有驗證機制，如果你擔心別人能從你的電腦上獲取相關信息的話，你應該設置防火牆規則來限制訪問。UI 很簡單，所以任何人看懂圖形並理解他們看到的結果，至少你會對它的快速安裝印象深刻。

## 安裝流程
---
### 可以採用 one line installation
> This method is **fully automatic on all Linux** distributions. FreeBSD and MacOS systems need some preparations before installing Netdata for the first time. Check the [FreeBSD](#freebsd) and the [MacOS](#macos) sections for more information.
```
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```
---
### 或是 Run Netdata in a Docker container
```
You can [Install Netdata with Docker](../docker/#install-netdata-with-docker).
```
---
### 我採用 Install Netdata on Linux manually
Install the packages for having a **basic Netdata installation** (system monitoring and many applications, without  `mysql` / `mariadb`, `postgres`, `named`, hardware sensors and `SNMP`):

```sh
curl -Ss 'https://raw.githubusercontent.com/netdata/netdata-demo-site/master/install-required-packages.sh' >/tmp/kickstart.sh && bash /tmp/kickstart.sh -i netdata

```

Install all the required packages for **monitoring everything Netdata can monitor**:

```sh
curl -Ss 'https://raw.githubusercontent.com/netdata/netdata-demo-site/master/install-required-packages.sh' >/tmp/kickstart.sh && bash /tmp/kickstart.sh -i netdata-all
```
安裝完後 Netdata 即自動啟動，檢查方式 ps aux|grep netdata
---
### 啟用 netdata，並輸入
```
https://localhost:19999
```
即可連接 netdata 監控頁面，如下
![image](https://github.com/P86071244/Netdata_demo1/blob/master/netdataSystemOverview.png)

若要停止服務，可輸入
```
systemctl stop netdata
```
或直接砍掉行程
```
sudo killall netdata
```
想重新啟動的話
```
systemctl start netdata
```
或可以直接執行主程式，預設目錄在 /usr/sbin 下面
```
sudo ./netdata
```
設定開機時自動帶起服務
```
systemctl enable netdata
```
或
```
sudo cp system/netdata-lsb /etc/init.d/netdata
sudo chmod +x /etc/init.d/netdata
sudo update-rc.d netdata defaults
```
取消開機時自動帶起服務
```
systemctl disable netdata
```
反安裝
```
sudo ./netdata-uninstaller.sh
```
刪除使用者及相關群組
```
sudo userdel netdata
sudo groupdel netdata
sudo gpasswd -d netdata docker
sudo gpasswd -d netdata adm
```
增加新功能及漏洞修補
```
sudo ./netdata-updater.sh
```
排程定時更新
```
./netdata_updater.sh
```
或
```
sudo ln -s /home/pi/netdata/netdata-updater.sh /etc/cron.daily/netdata-updater.sh
```
---
## 系統設定
```
sudo vi /etc/netdata/netdata.conf
```
分成
```
global - 服務的全域設定
plugins - 啟用或停用插件
plugin:NAME - 各個插件的設定
CHART_NAME - 各個圖表的設定
以預設值就已經可以正常的運作了，而這些參數可以視狀況修改：
update every = 1，每一秒更新一次
default port = 19999，預設通訊埠在 TCP 19999
bind to = *，不綁定 IPv4、IPv6 位址
disconnect idle web clients after seconds = 60，Web Client 閒置 60 秒後就踢掉
enable web responses gzip compression = yes，啟用網頁 GZip 壓縮功能
```
## References
```
[](https://github.com/netdata/netdata/blob/v1.14.0/packaging/installer/README.md)
[](https://github.com/netdata/netdata/blob/v1.14.0/docs/GettingStarted.md)
[](https://ithelp.ithome.com.tw/articles/10192159)
[](https://tpu.thinkpower.com.tw/tpu/articleDetails/708)
