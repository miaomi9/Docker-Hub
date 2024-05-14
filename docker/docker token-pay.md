### token-pay支付 虚拟货币支付

#### 创建配置文件
```
sudo touch /home/appsettings.json /etc/TokenPay.db
```

#### 写入`appsettings.json`配置
```
{
  "Serilog": {
    "MinimumLevel": {
      "Default": "Information",
      "Override": {
        "Microsoft": "Warning",
        "Microsoft.Hosting.Lifetime": "Information"
      }
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DB": "Data Source=|DataDirectory|TokenPay.db; Pooling=true;"
  },
  "TRON-PRO-API-KEY": "xxxxxx-xxxx-xxxx-xxxxxxxxxxxx", // 避免接口请求频繁被限制，此处申请 https://www.trongrid.io/dashboard/keys
  "BaseCurrency": "CNY", //默认货币，支持 CNY、USD、EUR、GBP、AUD、HKD、TWD、SGD
  "Rate": { //汇率 设置0将使用自动汇率
    "USDT": 0,
    "TRX": 0,
    "ETH": 0,
    "USDC": 0
  },
  "ExpireTime": 1800, //单位秒
  "UseDynamicAddress": false, //是否使用动态地址，设为false时，与EPUSDT表现类似；设为true时，为每个下单用户分配单独的收款地址
  "Address": { // UseDynamicAddress设为false时在此配置TRON收款地址，EVM可以替代所有ETH系列的收款地址，支持单独配置某条链的收款地址
    "TRON": [ "TLxxxxxxxxxxxxxxxxxxx" ],
    "EVM": [ "0x9966xxxxxxxxxxxxxxxx" ]
  },
  "OnlyConfirmed": false, //默认仅查询已确认的数据，如果想要回调更快，可以设置为false
  "NotifyTimeOut": 3, //异步通知超时时间
  "ApiToken": "666666", //异步通知密钥，请务必修改此密钥为随机字符串，脸滚键盘即可
  "WebSiteUrl": "http://token-pay.xxxxx.com", //配置服务器外网域名
  "Collection": {
    "Enable": false,
    "UseEnergy": true,
    "RetainUSDT": true, //归集USDT时是否保留0.000001，用于降低用户下次支付的成本
    "CheckTime": 1, //归集任务运行间隔，默认1小时运行一次，单位：小时
    "MinUSDT": 0.1, //只归集USDT余额大于此金额的地址
    "NeedEnergy": 31895, //归集USDT所需能量
    "EnergyPrice": 420, //波场当前能量单价
    "Address": "TLxxxxxxxxxxxxxxxxxxx" //归集收款地址
  },
  "Telegram": {
    "AdminUserId": 12345678, // 你的账号ID，查询机器人https://t.me/getidsbot
    "BotToken": "1234567890:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA" //从https://t.me/BotFather 创建机器人时，会给你BotToken
  },
  "RateMove": { //汇率微调，支持设置正负数，仅支持两位小数
    "TRX_CNY": 0,
    "USDT_CNY": 0
  }
}
```

## 运行
```
docker run -d --restart always -p 5000:80 -v /home/appsettings.json:/app/appsettings.json -v /etc/TokenPay.db:/app/TokenPay.db dapiaoliang666/token-pay:latest
```

然后将外网域名反代到`5000`端口


如果需要重新部署需要清空`/etc/TokenPay.db`文件里的内容


#### [官方地址](https://github.com/LightCountry/TokenPay)