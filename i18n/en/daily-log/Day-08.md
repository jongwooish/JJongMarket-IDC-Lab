# Day 08 - 2026.10.23 (Fri) The shop, and a day of errors

[한국어](../../../daily-log/Day-08.md) | **English** | [日本語](../../ja/daily-log/Day-08.md) | [简体中文](../../zh-CN/daily-log/Day-08.md) | [Español](../../es/daily-log/Day-08.md) | [Français](../../fr/daily-log/Day-08.md)

## What I did today

- Morning: put up a simple HTML shop (v1), but all Korean text showed as boxes on CLIENT
- The design felt too plain, so I rebuilt the shop (v2): yellow theme, 160 products, 800 reviews, search, cart, orders, simulated delivery, seller listings, card registration, membership, ad pop-ups
- Night: deployed v2 and fixed the 500 error, broken detail page, and sign-up error one by one

![v1](../../../docs/images/troubleshooting/01-hangul-broken.png)

## Where I got stuck

| Symptom | Cause | Fix |
| --- | --- | --- |
| Korean shown as boxes | No Korean font on CLIENT | `fonts-noto-cjk` |
| 500 error | `inc` folder sent via Windows scp was 700 | `chmod -R a+rX` |
| Broken detail page, sign-up error | `php-mbstring` missing | Install + fallback functions |
| Checkout fields empty | `header.php` and the page both used the `$f` variable | Renamed to `$flash_msg` |

The 500 error ran fine as root with `php index.php` but gave Permission denied with `runuser -u www-data`, which told me it was a permission problem. It was my first time reproducing an issue by switching the running user.

![permission](../../../docs/images/troubleshooting/03-permission-denied.png)

Details in [troubleshooting.md](../docs/troubleshooting.md)

## Thoughts

Today took the longest. Still, after finding and fixing each cause one by one, it felt great to see the shop running like a real shop.
