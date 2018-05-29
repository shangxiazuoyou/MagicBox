# 支付宝相关

###### Android检测手机支付宝客户端是否安装
代码如下：
```
/**
 * 检测是否安装支付宝客户端
 *
 * @param context 上下文
 * @return 是否安装支付宝客户端
 */
public static boolean checkAliPayInstalled(Context context) {
    Uri uri = Uri.parse("alipays://platformapi/startApp");
    Intent intent = new Intent(Intent.ACTION_VIEW, uri);
    ComponentName componentName = intent.resolveActivity(context.getPackageManager());
    return componentName != null;
}
```
