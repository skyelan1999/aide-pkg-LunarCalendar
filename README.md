# LunarCalendar · Aide 农历日历

Aide `lunar-calendar` 插件的独立分发仓库，版本 **1.0.0**，MIT 许可。源码来自 [Aide 4dc9a7b0](https://github.com/skyelan1999/aide/tree/4dc9a7b0b01fef6ac3550b0b0fcf97db42d45f94/plugins/lunar-calendar)，运行代码及安装 ZIP 与已有市场 1.0.0 保持一致。

## 能力

`get_lunar_date({date?})` 是可执行的只读工具。可传 `YYYY-MM-DD` 公历日期；省略日期时，按 **Asia/Shanghai 中国标准时间**确定今天。返回公历日期、农历年月日、闰月标记、干支年、生肖和易读农历日期。

例如，传入 `{"date":"2024-02-10"}` 返回：

```json
{
  "gregorianDate": "2024-02-10",
  "lunarYear": 2024,
  "lunarMonth": "正月",
  "isLeapMonth": false,
  "lunarDay": 1,
  "lunarDayName": "初一",
  "ganzhiYear": "甲辰",
  "zodiac": "龙",
  "lunarDate": "甲辰年正月初一",
  "timeZone": "Asia/Shanghai"
}
```

换算由 Node.js Intl/ICU 的 Chinese calendar 数据在本地完成，不访问网络、不读写工作区，也没有 npm 运行依赖。默认日期依据 **Aide 插件运行环境**的系统时钟，即使使用 SSH 工作区也不是远程设备时钟；当天日期始终按中国标准时间解释，与浏览器或容器默认时区无关。

无效日期或格式会返回错误。支持范围和换算数据取决于运行环境的 ICU；运行时缺少中国农历数据会明确报错。插件提供的是干支**年**。Aide 普通会话可通过现有插件工具机制调用，小秘的农历工具白名单需要宿主支持。插件没有独立 UI 或工作台。

## 安装

需要支持插件导入的 Aide（市场兼容基线 `0.1.17.0 RC2`）及含中国农历 ICU 数据的 Node.js 插件运行环境。平台声明为 Windows、macOS 和 Linux；本次实际验证环境为 Linux，未实测 Windows/macOS。

从 [v1.0.0 Release](https://github.com/skyelan1999/aide-pkg-LunarCalendar/releases/tag/v1.0.0) 说明中的固定提交链接下载 `aide-pkg-LunarCalendar-v1.0.0.zip` 和 `SHA256SUMS`，校验后通过 Aide 插件管理导入；也可在[官方插件市场](https://skyelan1999.github.io/aide-pkg-shop/)搜索 `lunar-calendar`。

```sh
sha256sum -c SHA256SUMS
```

已安装同 ID、同版本的用户无需重复安装。ZIP 保留原包的 `LICENSE`、`index.js`、`manifest.json` 和 `test.js`，不重新打包。安装包托管于仓库 `packages/v1.0.0/`，Release 提供固定提交下载链接，没有另行上传二进制附件；市场分发相同字节。

## 验证

```sh
node --check index.js
node scripts/verify.cjs
```

验证脚本在临时目录中提供原测试需要的最小注册表，运行未修改的 `test.js`，覆盖春节、闰二月、干支生肖、默认日期及无效输入。发布时另用 Aide 原生插件宿主验证入口、可执行工具 surface 和实际调用，并运行 Aide/小秘农历工具回归。精确结果见 `docs/release-verification.json`。

SHA-256 用于完整性校验，不是发布者签名；可信 Node 插件机制不是权限沙箱。
