# Системные и скрытые настройки

Редкие и скрытые настройки можно менять напрямую в файле config.xml

(/sdcard/Android/data/hobdrive.android/files/hobdrive)


### интервалы переподключения

```
<reconnect-timeout-short>5000</reconnect-timeout-short>

<reconnect-timeout-long>10000</reconnect-timeout-long>
```

Короткий и длинный интервалы в милисекундах для повторного переподключения к ELM адаптеру.

Короткий - для активного режима, Длинный - когда приложение в фоне