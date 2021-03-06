# mini-ci

[![license](https://img.shields.io/npm/l/@vyron/mini-ci)](https://github.com/VFiee/mini-ci)
[![npm version](https://img.shields.io/npm/v/@vyron/mini-ci.svg)](https://github.com/VFiee/mini-ci)

mini-ci 基于[miniprogram-ci](https://www.npmjs.com/package/miniprogram-ci)开发,用于以配置管理多个小程序项目.

## Table of Contents

- [安装](#安装)
  - [yarn](#yarn)
  - [npm](#npm)
- [配置](#配置)
- [示例](#示例)
- [使用](#使用)
  - [上传](#上传)
  - [预览](#预览)
  - [构建 npm](#构建npm)
  - [获取 sourcemap](#获取sourcemap)
  - [全局配置](#全局配置)
    - [设置项目配置](#设置项目配置)
    - [获取项目配置列表](#获取项目配置列表)
    - [获取项目配置详情](#获取项目配置详情)
    - [删除项目配置](#删除项目配置)
    - [获取或设置默认配置](#获取或设置默认配置)
    - [导出项目配置](#导出项目配置)
    - [清空项目配置](#清空项目配置)
- [参考文档](#参考文档)
- [维护者](#维护者)
- [如何贡献](#如何贡献)
- [使用许可](#license)

## 安装

### yarn

```bash
yarn global add @vyron/mini-ci
```

### npm

```bash
npm install @vyron/mini-ci -g
```

## 配置

[参考微信文档](https://developers.weixin.qq.com/miniprogram/dev/devtools/ci.html)

### 项目配置

|         key         |                      默认值                       |                 env                 |    类型    | 必填 |                                       说明                                        |
| :-----------------: | :-----------------------------------------------: | :---------------------------------: | :--------: | :--: | :-------------------------------------------------------------------------------: |
|       `appid`       |  当前项目 `project.config.json` 的 `appid` 字段   |           `appid` / `id`            |  `string`  |  否  |                             小程序或小游戏的 `appid`                              |
|    `projectPath`    |                        无                         |      `projectPath` / `proPath`      |  `string`  |  是  |                                   项目源码路径                                    |
|  `privateKeyPath`   |                        无                         |    `privateKeyPath` / `priPath`     |  `string`  |  是  |                            小程序或小游戏代码上传密钥                             |
|       `type`        |                   `miniProgram`                   |            `type` / `t`             |  `string`  |  否  | 当前项目类型,有效值 `miniProgram`/`miniProgramPlugin`/`miniGame`/`miniGamePlugin` |
|      `ignores`      |                        无                         |          `ignores` / `ig`           | `string[]` |  否  |                                  指定忽略的规则                                   |
|      `version`      | 项目及其上级三层目录的`package.json`里的`version` |                `ver`                |  `string`  |  是  |                                   自定义版本号                                    |
|       `desc`        |                   当前本地时间                    |            `desc` / `d`             |  `string`  |  否  |                                  自定义备注信息                                   |
|       `robot`       |                         1                         |            `robot` / `b`            |  `number`  |  否  |             指定 CI 机器人,可选值`1~30` 上传成功后将显示:ci 机器人 1              |
|   `qrcodeFormat`    |                    `terminal`                     |  `qrcodeFormat` / `qrFormat`/`qrf`  |  `string`  |  否  |            预览返回二维码文件格式,可选值: `image`/`base64`/`terminal`             |
| `qrcodeOutputDest`  |                     当前项目                      | `qrcodeOutputDest`/`qrDest` / `qrd` |  `string`  |  否  |           当`qrcodeFormat`为`image`或`base64`时,文件默认保存到当前项目            |
|     `pagePath`      |                        无                         |       `pagePath` / `pp` / `p`       |   string   |  否  |                                   预览页面路径                                    |
|    `searchQuery`    |                        无                         |     `searchQuery` / `sq` / `q`      |   string   |  否  |                                 预览页面启动参数                                  |
| `sourceMapSavePath` |             当前项目下`soucemap.zip`              |     `sourceMapSavePath` / `sp`      |   string   |  否  |                             保存 sourcemap 的绝对路径                             |

### 编译配置

|       key        | 默认值 |       env        |   类型    | 必填 |       说明        |
| :--------------: | :----: | :--------------: | :-------: | :--: | :---------------: |
|      `es6`       |   无   |      `es6`       | `boolean` |  否  |     启用 es6      |
|      `es7`       |   无   |      `es7`       | `boolean` |  否  |     启用 es7      |
|     `minify`     |   无   |     `minify`     | `boolean` |  否  |   启用压缩代码    |
|  `codeProtect`   |   无   |  `codeProtect`   | `boolean` |  否  |   启用代码混淆    |
|    `minifyJS`    |   无   |    `minifyJS`    | `boolean` |  否  |    启用压缩 JS    |
|   `minifyWXML`   |   无   |   `minifyWXML`   | `boolean` |  否  |   启用压缩 XWML   |
|   `minifyWXSS`   |   无   |   `minifyWXSS`   | `boolean` |  否  |   启用压缩 WXSS   |
| `autoPrefixWXSS` |   无   | `autoPrefixWXSS` | `boolean` |  否  | 启用自动补全 WXSS |

## 示例

```jsonc
// project
"type":"miniProgram",
"appid":"93457450667",
"privateKeyPath":"dist/weapp",
"privateKeyPath":"private.key",
// settings
"setting":{
    "es6":true,
    "es7":true,
    "minify":true,
    "codeProtect":true,
    "minifyJS":true,
    "minifyWXML":true,
    "minifyWXSS":true,
    "autoPrefixWXSS":true,
}
// others
"robot":10,
"qrcodeFormat":"image",
"qrcodeOutputDest":"preview.jpg",
"pagePath":"pages/users/index",
"searchQuery":"user_id=87504653",
"sourceMapSavePath":"sourcemap.zip"
```

## 使用

### 上传

```bash
mini-ci upload -h
mini-ci upload --ver "1.0.0"
```

### 预览

```bash
mini-ci preview -h
mini-ci preview
```

### 构建 npm

```bash
mini-ci build -h
mini-ci build
```

### 获取 sourcemap

```bash
mini-ci sourcemap -h
mini-ci sourcemap
```

### 全局配置

```bash
# 查看config帮助信息
mini-ci config -h
```

#### 设置项目配置

```bash
# 设置项目配置
mini-ci config set --name test_set_project --path /Users/vyron/mini/mini-ci.json --default
```

#### 获取项目配置列表

```bash
mini-ci config ls
```

#### 获取项目配置详情

```bash
mini-ci config get --name test_set_project
```

#### 删除项目配置

```bash
mini-ci config delete --name test_set_project
```

#### 获取或设置默认配置

```bash
# 获取默认配置
mini-ci config default

# 设置为默认配置  项目名(test_set_project)必须已存在
mini-ci config default --name test_set_project
```

#### 导出项目配置

```bash
# 如果不指定导出项目名,导出默认项目配置,默认导出路径为当前项目 export-mini-ci.json
mini-ci config export --name test
```

### 清空项目配置

```bash
# 清空所有配置
mini-ci config clear

# 展示当前配置列表
mini-ci config ls
```

## 参考文档

[小程序开发者文档](https://developers.weixin.qq.com/miniprogram/dev/devtools/ci.html)  
[miniprogram-ci(npm)](https://www.npmjs.com/package/miniprogram-ci)

## 维护者

[Vyron](https://github.com/VFiee)

## 如何贡献

非常欢迎你的加入！[提一个 Issue](https://github.com/RichardLitt/standard-readme/issues/new) 或者提交一个 Pull Request。

## 使用许可

[MIT](LICENSE) © VFiee
