我的账户  页面，里面有链接，需要更改IP域名。

### 一
修改了 You must be logged in to checkout.
当没有登录，加了商品到购物车，然后去结算时，就会出现，让你先登录再结算。 修改下面的文件。
```
/var/www/html/wordpress/wp-content/themes/estore/woocommerce/checkout# vi form-checkout.php 
28行
```
注意，这里需要 修改登录 的链接。 

#### estore  修改 下单界面
```
/var/www/html/wordpress/wp-content/themes/estore/woocommerce/checkout#  vi form-billing.php 
```



### 二（这里修改了 woocommerce 插件)
我的账户登录界面修改
```
/var/www/html/wordpress/wp-content/plugins/woocommerce/templates/myaccount# vi form-login.php
```
修改了用户 “账户详情” 修改界面！
```
/var/www/html/wordpress/wp-content/plugins/woocommerce/templates/myaccount# vi form-edit-account.php
```

### 三
```
/var/www/html/wordpress/wp-content/themes/estore# vi header.php
```
这个文件里 修改了登录链接

### 四 去掉了 ThemeGrill Demo Importer 推荐

```
/var/www/html/wordpress/wp-content/themes/estore# vi ./inc/tgm-plugin-activation/tgmpa-estore.php
```
