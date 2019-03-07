```
export DP_Id="xxxx"
export DP_Key="xxxxxxx"
acme.sh --issue --dns dns_dp -d demo.starsing.cn -d *.demo.starsing.cn

mkdir -p /data/web/ssl/demo.starsing.cn

acme.sh  --installcert  -d  demo.starsing.cn   \
        --key-file   /data/web/ssl/demo.starsing.cn/demo.starsing.cn.key \
        --fullchain-file /data/web/ssl/demo.starsing.cn/demo.starsing.cn.cer \
        --reloadcmd  "chown -R data.data /data/web/ssl/demo.starsing.cn" \
        --reloadcmd  "/etc/init.d/nginx force-reload"
```

crontab -e
```
00 00 * */1 *"/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" --force
```

