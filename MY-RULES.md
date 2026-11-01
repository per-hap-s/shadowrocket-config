# 个人层说明（fork 自 DuskWander87/shadowrocket-config）

## 分工

| 位置 | 谁维护 | 说明 |
|---|---|---|
| `shadowrocket.conf` 的 [General] / [URL Rewrite] / 上游规则行 | 上游 | 自动同步，不要手改 |
| `shadowrocket.conf` 的「个人层」两行 | 本仓库 | 指向下面两个表 |
| `rules/Personal-Direct.list` | 你 | 直连补充规则 |
| `rules/Personal-Reject.list` | 你 | 拦截补充规则 |
| `rules/ChinaDirect.list`、`rules/Reject.list` | 上游 | conf 里引用的是**作者仓库**的地址，作者更新即时生效，不需要同步 |
| `.github/workflows/sync-upstream.yml` | 你 | 每 6 小时把上游 main merge 进来 |

个人表和上游表分开放，所以日常只需改 `rules/Personal-*.list`，conf 那边不碰，merge 冲突概率接近 0。

## 加规则

直连：往 `rules/Personal-Direct.list` 追加一行，例如 `DOMAIN-SUFFIX,example.com`
拦截：往 `rules/Personal-Reject.list` 追加一行

表内**不写策略**，策略由 conf 里 RULE-SET 那一行统一指定。所以直连和拦截必须分表。

`DST-PORT,123`（NTP 直连）也放在 Personal-Direct.list 里，语法与主配置一致。

## 生效

1. 改完 push
2. 手机：小火箭 → 配置 → 点这份配置 → 更新
3. rule 文件走 raw.githubusercontent.com（与上游风格一致），跟着代理一起拉，无 CDN 缓存延迟

## 同步失败怎么办

Actions 报红 = 上游改到了你改的同几行。本地：

```
git remote add upstream https://github.com/DuskWander87/shadowrocket-config.git  # 只需一次
git fetch upstream && git merge upstream/main
```

解决冲突后 push。因为个人规则都在独立文件里，正常不会冲突。
