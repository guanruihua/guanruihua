# version

## 查看包历史版本

```shell
# 查看包全部包历史版本
npm v [package-name] versions
# 查看包最新版本
npm v [package-name] version
```

## version

- `npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease | from-git]`
- 版本控制

- major：主版本号（大版本）(3.1.0 -->4.0.0)
- premajor：预备主版本 (1.1.0-0 变为 2.0.0-0)
- minor：次版本号（小更新）( 2.0.1 变为 2.1.0 )
- preminor: 预备次版本 (1.0.2-0 变为 1.1.0-0)
- patch：补丁号（补丁）(2.0.0- 0变为 2.0.0 / 2.0.0 变为 2.0.1)
- prepatch：预备补丁版本 (1.0.1-1 变为 1.0.2-0)
- prerelease：预发布版本

```shell
npm version patch  // 1.0.1 表示小的bug修复
npm version minor // 1.1.0 表示新增一些小功能
npm version major // 2.0.0 表示大的版本或大升级
npm version preminor // 1.1.0-0 后面多了个0，表示预发布
```

## `npm versino`

```shell
npm version

{
  npm: '6.14.10',
  ares: '1.18.1',
  brotli: '1.0.9',
  cldr: '40.0',
  icu: '70.1',
  llhttp: '6.0.4',
  modules: '93',
  napi: '8',
  nghttp2: '1.45.1',
  nghttp3: '0.1.0-DEV',
  ngtcp2: '0.1.0-DEV',
  node: '16.14.0',
  openssl: '1.1.1m+quic',
  tz: '2021a3',
  unicode: '14.0',
  uv: '1.43.0',
  v8: '9.4.146.24-node.20',
  zlib: '1.2.11'
}
```
