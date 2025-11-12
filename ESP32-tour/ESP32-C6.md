# ESP32-C6开发

## 环境(Windows)

我的是ESP32-C6 SuperMini，自带一个type-c接口，连接到Windows，我的是连到了`COM3`
- `idf.py`命令需要从VSCode底部状态栏中`Open ESP-IDF Terminal`进入

## 编译

跟随官方文档，基本是：

- 将底部icon的chip改为`esp32c6`
- idf.py build
- idf.py -p COM3 flash
