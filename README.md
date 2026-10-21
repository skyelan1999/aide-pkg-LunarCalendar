# 农历日历 · LunarCalendar 1.1.0

独立Aide插件，提供农历月历工作台和只读日期工具。代码MIT许可；香港天文台日期事实集另按政府开放资料条款署名，不声明为MIT或官方认证产品。

## 工作台

在插件列表中点击「农历工作台」。可查看公历/农历月历、闰月、生肖、农历年的干支和节气日期，支持日期输入、前后月份、今天、方向键/Home/End选日、桌面/窄屏和宿主明暗主题。工作台与Aide工具使用同一套离线日期数据，不调用模型。

## Aide工具

- `get_lunar_date({date?})`：严格公历YYYY-MM-DD；省略时按Aide主机时钟、Asia/Shanghai确定今天。保留原有gregorianDate、lunarYear、lunarMonth、isLeapMonth、lunarDay、lunarDayName、ganzhiYear、zodiac、lunarDate、timeZone，新增月份数字、天数与来源信息。
- `get_lunar_month({year?,month?})`：年月一起提供或一起省略，返回整月农历、闰月、星期定位及节气日期。

例如 `{"date":"2024-02-10"}` 为甲辰年正月初一；`{"date":"1917-03-23"}` 为闰二月初一；2033年闰十一月、2057年临近午夜朔的日期均按固定HKO历表口径处理。

## 日期口径与限制

支持1901-01-01至2099-12-31。1.1.0使用固定版本香港天文台公开历表的离线月份锚点，替代旧版依赖Node/ICU版本的中国农历转换。今天只省略参数才使用，空字符串、null、数字和无效日期明确报错，不静默退回今天。干支年和生肖以正月初一换年，不混用立春界限。来源及远期接近午夜的预测不确定性均可查询，详见[审计报告](docs/calendar-audit.md)。不提供官方调休安排，也不含宜忌预测。

## 安装和验证

需要Node.js 18+及支持protocol 1工作台桥的Aide。市场兼容基线0.1.17.3 RC1；旧宿主需升级才能打开工作台。支持Windows/macOS/Linux及SSH工作区，实际测试为Linux；数据在Aide宿主本地读取，不访问SSH设备。

从本仓库Releases下载1.1.0 ZIP与SHA256SUMS，或从官方商店升级；同ID升级保留用户设置。ZIP明确包含运行资源，无需npm安装、网络API或中国农历ICU数据。

```sh
node scripts/build.cjs
node scripts/verify.cjs
node test/calendar-audit.test.cjs
python3 scripts/package.py
```

逐日离线回归覆盖72,684天，与黄历插件使用相同数据指纹。`scripts/fetch-reference.py --cache /path/to/cache` 是开发者显式更新数据的工具，运行时不会联网。上游资料未来修订后应重新审计发布，不静默替换已安装数据。来源与权利说明见[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
