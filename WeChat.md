# 微信相关

###### Android判断手机是否安装微信客户端
网上有很多关于判断方法，无外乎两种：

一，通过判断手机中安装的应用的包名中，是否有符合微信的包名的。

二，通过集成微信的SDK后，使用SDK里的api方法进行判断。

经测试都有问题，即单独使用其中的一种方法都不能覆盖所有机型。如，使用微信SDK里提供的判断方法，在三星S7手机上始终返回false,不管你装没装微信。而使用包名的方法，则在华为的某一款手机上也始终返回false, 不管你装没装微信。所以采用二者结合的方式，先使用SDK里的方法判断一下，如果返回false，则继续使用判断包名的方法。代码如下：
```
private static IWXAPI api; // 相应的包，请集成SDK后自行引入
         
/**
* 判断微信客户端是否存在
*
* @return true安装, false未安装
*/
public boolean isWeChatAppInstalled(Context context) {
        iwxapi = WXAPIFactory.createWXAPI(context, Configs.WECHATAPP_ID);
        if (iwxapi.isWXAppInstalled() && iwxapi.isWXAppSupportAPI()) {
            return true;
        } else {
            final PackageManager packageManager = context.getPackageManager();// 获取packagemanager
            List<PackageInfo> pinfo = packageManager.getInstalledPackages(0);// 获取所有已安装程序的包信息
            if (pinfo != null) {
                for (int i = 0; i < pinfo.size(); i++) {
                    String pn = pinfo.get(i).packageName;
                    if (pn.equalsIgnoreCase("com.tencent.mm")) {
                        return true;
                    }
                }
            }
            return false;
        }
    }
```

以上代码在三星S7测试时，会先走SDK的判断，因为SDK的判断始终返回FALSE，因此会走到包名的判断，包名的判断在S7上才有用，如果手机上安装了微信，则返回true, 否则返回false。

在华为的某型号手机上（记不得了，测试时遇到过，印象很深，因为最开始的方法就是判断包名，在这款手机上就无效，后来改为使用SDK的判断方法实测有效），判断包名的方法无效，因此用以上方法判断时，先走SDK的判断方法，安装了微信，则返回true，否则返回false。
