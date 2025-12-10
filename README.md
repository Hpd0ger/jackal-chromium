# jackal-chromium

通过更改 Chromium 源码实现定制化爬虫需要，参考[chromium_for_spider](https://github.com/myvyang/chromium_for_spider)

## 魔改版本说明

<table>
  <tr>
    <th>diff 文件名称</th>
    <th>功能描述</th>
    <th>使用说明</th>
  </tr>
  <tr>
    <td rowspan="3">spider_1210.diff</td>
    <td>拦截location重定向</td>
    <td>设置 window.locationRelease 对象，置 true 时才允许浏览器执行跳转。对于爬虫类场景，需要控制页面采集进度并在适时通过 page.evaluate 设置locationRelease</td>
  </tr>
  <tr>
    <td>修改webdriver</td>
    <td>设置navigator.webdriver属性为undefined，防止基于webdriver属性的爬虫检测</td>
  </tr>
  <tr>
    <td>修改console.debug</td>
    <td>禁用console.debug执行，防止基于 console.debug 的爬虫检测</td>
  </tr>
</table>

## 编译
先按照[官方步骤](https://chromium.googlesource.com/chromium/src/+/main/docs/mac_build_instructions.md)，下载源码并准备好环境

下载完源码后执行如下命令
```bash
# 进入 Chromium 源码所在目录并切换到制定版本, ca233对应Chromium145.0.7560.0
cd src
git checkout ca233edc6900705165eeb4c652bbae2486de34fd
git apply /path/to/spider_version/spider_datetime.diff
# 复制args.gn到项目文件夹中, 按需更改
cp /path/to/spider_version/spider_version/args.gn ./
# 生产编译文件
gn gen out/Release
# 开始编译
autoninja -C out/Release chrome
```

**构建产物**
* Mac上可执行文件在src/out/Release/Chromium.app/Contents/MacOS/Chromium
* Ubuntu上可执行文件在src/out/Release/chrome