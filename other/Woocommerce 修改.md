### 一
修改了 You must be logged in to checkout.
当没有登录，加了商品到购物车，然后去结算时，就会出现，让你先登录再结算。 修改下面的文件。
```
/var/www/html/wordpress/wp-content/themes/estore/woocommerce/checkout# vi form-checkout.php 
28行
```
注意，这里需要 修改登录 的链接。 

### 二
我的账户登录界面修改
```
/var/www/html/wordpress/wp-content/plugins/woocommerce/templates/myaccount# vi form-login.php
```
