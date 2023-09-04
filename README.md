# Zotero 插件合集

本仓库提供了若干 Zotero 插件的信息及其 xpi 包，尝试在 Zotero 官方插件商店建立前，提供集中的插件商店服务。

## 访问

- GitHub Pages: <https://northword.github.io/zotero-plugins/>
- Netlify: 待部署
- Vercel: 待部署
- Gitee: 待部署

## 贡献

### 插件信息

插件信息保存在 `src/plugins.json`，数据格式如下：

```ts
interface PluginInfo {
  name: string;                   // 插件名称
  repo: string;                   // 插件仓库：northword/zotero-format-metadata，前后均无 `/`
  releases: PluginReleaseInfo[];  // 插件的发行版信息
  description?: string;

}

interface PluginReleaseInfo {
  targetZoteroVersion: string;             // 当前发行版针对的 Zotero 版本
  tagName: "latest" | "pre" | "string";    // 当前发行版发布的类型——latest：最新正式发布，pre：最新预发布，string：发布对应的 git.tag_name 
}
```

对于每一个插件，只有必填项需要填写在 `src/plugins.json` 中，其余字段脚本运行时可以获取。

### 构建过程

`src/index.ts` 为主要逻辑脚本，它执行如下操作：

- 遍历上述 JSON 文件，获取每一个插件的基本信息和发行版，将获取到的信息保存在 `dist/plugins.json`
- 同时将 XPI 包保存在 `dist/xpi/${github.release.asset.id}.xpi`
- 根据得到的信息，渲染为 Markdown 表格，写入 `dist/plugins.md`
- 将 docsify 模板从 `docs/` 复制到 `dist/`

GitHub Action Bot 定时运行 `src/index.ts` 脚本，执行上述步骤，并将 `dist` 部署到 `gh-page` 分支。

### 开发

```bash
# 克隆仓库
clone https://github.com/northword/zotero-plugins.git zotero-plugins
cd zotero-plugins

# 安装依赖
npm install

# 运行构建脚本
npm build

# 启动网页服务器
npm website
```

## 致谢

本项目使用了如下代理或公共 CDN 服务：

- GitHub 代理：<https://github.com/hunshcn/gh-proxy>
- JsDeliver：<https://www.jsdelivr.com/>

本项目是对 [l0o0/ZoteroPlugins](https://github.com/l0o0/ZoteroPlugins) 的 Typescript 重新实现。

## 协议

MIT License
