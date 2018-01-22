# Android开发总结 #

---

## Android O(8.0)适配 ##

---

### 通知适配 ###
Android O新增通知渠道，其允许为要显示的每种通知类型创建用户可自定义的渠道。用户界面将通知渠道称之为通知类别。将targetSdkVersion提到26以上的话，就必须使用NotificationCompat.Builder(context, channelId)（有两个参数的）并且channelId不能为null。
**1.创建NotificationChannel**
如果你需要发送属于某个自定义渠道的通知，你需要在发送通知前创建自定义通知渠道，示例如下：

```
//ChannelId为"1",ChannelName为"Channel1"
NotificationChannel channel = new NotificationChannel("1","Channel1", NotificationManager.IMPORTANCE_DEFAULT);
channel.enableLights(true); //是否在桌面icon右上角展示小红点
channel.setLightColor(Color.GREEN); //小红点颜色
channel.setShowBadge(true); //是否在久按桌面图标时显示此渠道的通知
notificationManager.createNotificationChannel(channel);
```

**2.向NotificationChannel发送通知**

```
public static void showChannel1Notification(Context context) {
    int notificationId = 0x1234;
    Notification.Builder builder = new Notification.Builder(context,"1"); //与channelId对应
    //icon title text必须包含，不然影响桌面图标小红点的展示
    builder.setSmallIcon(android.R.drawable.stat_notify_chat)
            .setContentTitle("xxx")
            .setContentText("xxx")
            .setNumber(3); //久按桌面图标时允许的此条通知的数量
    NotificationManager notificationManager = (NotificationManager) context.getSystemService(Context.NOTIFICATION_SERVICE);
    notificationManager.notify(notificationId, builder.build());
}
```

**3.删除NotificationChannel**

```
NotificationChannel mChannel = mNotificationManager.getNotificationChannel(id);
mNotificationManager.deleteNotificationChannel(mChannel);
```
