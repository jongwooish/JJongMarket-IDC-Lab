# Day 08 - 2026.10.23 (周五) 商城，以及错误之日

[한국어](../../../daily-log/Day-08.md) | [English](../../en/daily-log/Day-08.md) | [日本語](../../ja/daily-log/Day-08.md) | **简体中文** | [Español](../../es/daily-log/Day-08.md) | [Français](../../fr/daily-log/Day-08.md)

## 今天做了什么

- 上午: 上线了简单的 HTML 商城(v1)，但在 CLIENT 上韩文全部显示为方块
- 设计实在太简陋，于是重做了商城(v2): 黄色主题、160 件商品、800 条评价、搜索、购物车、下单、模拟配送、商品上架、银行卡登记、会员、广告弹窗
- 晚上: 上线 v2 后，依次解决了 500 错误、详情页显示异常、注册错误

![v1](../../../docs/images/troubleshooting/01-hangul-broken.png)

## 卡住的地方

| 症状 | 原因 | 解决 |
| --- | --- | --- |
| 韩文显示为方块 | CLIENT 上没有韩文字体 | `fonts-noto-cjk` |
| 500 错误 | 用 Windows scp 发送的 `inc` 文件夹权限为 700 | `chmod -R a+rX` |
| 详情页异常、注册错误 | 缺少 `php-mbstring` | 安装 + 替代函数 |
| 订单页输入框为空 | `header.php` 与页面共用 `$f` 变量 | 改名为 `$flash_msg` |

500 错误在用 root 执行 `php index.php` 时正常，用 `runuser -u www-data` 执行时却出现 Permission denied，由此判断是权限问题。这是我第一次用切换执行用户的方法来复现问题。

![permission](../../../docs/images/troubleshooting/03-permission-denied.png)

详情见 [troubleshooting.md](../docs/troubleshooting.md)

## 感想

今天花的时间最长。不过一个个找到原因修好之后，看到商城像真正的商城一样运行，很有成就感。
