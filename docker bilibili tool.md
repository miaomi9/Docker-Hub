B站自动执行任务的工具



```
docker run -d --name="bili" \
    -v /bili/Logs:/app/Logs \
    -e Ray_DailyTaskConfig__Cron="0 15 * * *" \
    -e Ray_LiveLotteryTaskConfig__Cron="0 22 * * *" \
    -e Ray_UnfollowBatchedTaskConfig__Cron="0 6 1 * *" \
    -e Ray_VipBigPointConfig__Cron="7 1 * * *" \
    ghcr.io/raywangqvq/bilibili_tool_pro
```





#### 查看实时日志
```
docker logs -f bili
```



####  运行扫码进行登录
```
docker exec -it bili bash -c "dotnet Ray.BiliBiliTool.Console.dll --runTasks=Login"
```





---