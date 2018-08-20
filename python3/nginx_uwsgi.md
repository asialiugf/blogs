### uwsgi开机问题
```python
export LD_LIBRARY_PATH=/root/anaconda3/pkgs/openssl-1.0.2o-h20670df_0/lib:/root/anaconda3/pkgs/icu-58.2-h9c2bf20_1/lib
 1067  mv libstdc++.so.6.0.19 libstdc++.so.6.0.19.bak
 1068  ln -s /root/anaconda3/lib/libstdc++.so.6.0.24 libstdc++.so.6
 1069  rm libstdc++.so.6
 1070  ln -s /root/anaconda3/lib/libstdc++.so.6.0.24 libstdc++.so.6
```
