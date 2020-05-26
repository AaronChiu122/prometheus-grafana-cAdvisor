# Setup envrionment
- Install docker

# Code structure

```
grafana/
    provisioning/
        dashboards/
            *.json                      // dashboard 模板，可存放自己製作或既有的Dashboard
            dashboard.yml               // 其設定
        datasources/
            datasources.yml             // 資料庫設定與認證
    config.monitoring                   // grafana 設定 (登入密碼)，預設密碼於檔案中，可修改登入密碼

prometheus/
    prometheus.yml                      // prometheus 與 其他容器(cAdvisor, node-exporter)的關聯設定

docker-compose.yml                      // docker-compose
```

# 使用方式

- start: `docker-compose up`
- stop: `docker-compose down`


# 備註

- 如果發現 Dashboard 沒資料或很久才動一次，可再右上角設定"Last 1 hour"，以及最右邊欄位設定每5秒更新一次

- 如果發現 Dashboard 出現"!"驚嘆號，表示無資料或Query語法錯誤而導致找不到資料

- 欲下載已配置好的 Dashboard，於Dashboard右上角點選Share Dashboard -> Export -> Save to file

- docker-compose
    - docker-compose版本不可超過3.6，若使用3.7版在 Staging server 上會出錯

- Alert
    - 須先設定 "Notification channels" 再到Dashboard中將欲通知的channle加入alert中
    - 若欲使用slack通知，必須給Url與Token，其餘可依照需求設定，最後再進行"Send Test"測試
    - 接下來，再Dashboard中進入panel編輯，可點選左下角的鈴鐺並新增Alert，Condition可設定觸發條件，並於下方 "Send to"的地方添加channel，其餘設定可依照需求配置



# Monitors容器介紹

## Grafana:

- 除了以圖形化介面顯示數據之外，也提供Alert可發送訊息至Slack, Email, Teams…等等。
- 省去了Alertmanager這個container


## cAdvisor:

- 可以查看每個 ”Container” 的使用資訊：
    - 目前在運行中的容器數量
    - Memory usage
    - CPU usage
    - Network input/output
    - File System write/read


## Node-exporter:

- 可以看 ”主機” 的使用資訊：
    - CPU核心數
    - 主記憶體容量
    - Memory usage
    - CPU usage
    - Filesystem usage

## Prometheus:

- 蒐集/儲存上述數據給Grafana使用