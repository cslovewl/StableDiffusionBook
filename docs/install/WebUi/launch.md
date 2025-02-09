# 启动

>TODO Rewrite

首先必须下载一个模型（比如 [Any3](https://huggingface.co/andite/anything-v4.0/tree/main)），你可以在 [civitai](https://civitai.com/) 选择一个。界面顶部的菜单用于选择模型。


你可以在 `stable-diffusion-webui/models/Stable-diffusion/` 中放置你的大模型。

在 `/stable-diffusion-webui/models/Lora/` 中放入你的 Lora 人物卡模型.

## 运行并安装程序

Win 编辑并运行 `webui.bat`

Linux 用户运行 `launch.py` 或 `webui.py` ，前者会自动安装（新）环境。

## 尝试生成你的第一幅图片

这里给出一个实例来供你完成测试。

- 第一个输入框里写关于期望结果的词汇

可以使用 颜文字 和 emoji，使用 () 来增强权重，比如

```
((masterpiece)), best quality, illustration, 1 girl, beautiful,beautiful detailed sky, catgirl,beautiful detailed water, cinematic lighting, dramatic angle, Grey T-Shirt, (few clothes),(yuri),(medium breasts),white hair,cat ears,masturbation,bare shoulders ,(gold eyes),wet clothes
```

- 第二个输入框书写不希望出现的结果

指定需要过滤什么标签，比如

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```

**第一次调整参数**

选定分辨率参数，越大生成耗时越长。

生成后的结果结尾类似如下
```
Steps: 28, Sampler: Euler a, CFG scale: 7, Seed: 2706937631, Size: 1024x512, Model hash: 925997e9
```
在没有加载风格模型的情况下，通过一样的参数和 Seed(-1 就是随机），可以生产一样的图像，可以用于微调！

一起出现的还有 GPU 负载信息

```
Time taken: 33.97s
Torch active/reserved: 1975/2144 MiB, Sys VRAM: 7890/8134 MiB (93.61%)
```

**快捷设置**

不想来回设置 Clip 参数？

添加 `sd_hypernetwork` 和 `CLIP_stop_at_last_layers` 到设置页面的 `Quicksettings list` ，点击页面顶部的按钮保存，重新启动 webui，你就可以在 Ui 顶部看到一个快速切换选项啦～

!!! tip
    需要添加更多快捷设置？

    有 Python 代码阅读能力的话则可在 [此处](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/shared.py) 源码中找到各设置定义的名称。
    
    它们通常以将对象 `OptionInfo` 的实例作为值的字典的键存在。

## 中文/本地化

本地化插件已经集成进插件管理器中。本地化文件是在 `localizations` 文件夹下的Json文件。

**创建本地化文件**

转到设置并单击 `Download localization template` 底部的按钮，下载一个可以编辑的本地化模板。

## 共享链接

- 使用 `--share` 选项来在线运行！

你会得到一个 `xxx.app.gradio` 共享链接，另外你可以使用参数为所述 `gradio` 共享实例设置身份验证：`--gradio-auth username:password`

!!! danger "RCE"

    当使用 `--share` 参数时，你的实例就可以在公开互联网被访问了。

    但是最近有人报告了一个可能是危险的 [代码漏洞](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2571)

    所以，你可以使用参数为 `gradio` 共享实例设置身份验证：`--gradio-auth username:password` （可提供以逗号分隔的多组用户名和密码）

    例子
    `--share --gradio-auth admin:admin,user1:user_password`

    使用该例子将会创建两个用户，一个是账号密码为 admin 的用户，另外一个是账号为 user1, 密码为 user_password 的用户

    如果攻击者可以访问 ui，他们可能能够远程运行 python 脚本。

    10/30 社区报告：有人在扫描公开的 WebUi

    11/1 社区反映：共享链接可能会导致风险，**攻击者可以访问系统上的所有文件。**

你可以通过添加 `--freeze-settings`  启动参数来锁定设置，防止他人编辑。

- 端口转发

参数 `--listen` 使服务器监听网络连接。

这将允许本地网络上的计算机访问 UI，如果配置端口转发，公网上的计算机也可以访问（当然你得有公网。

注意，监听 `0.0.0.0` 就意味着对本地网卡的所有 ipv4 监听，局域网访问请使用 `localhost` 替换 `0.0.0.0`

- 监听端口

添加参数 `--port xxxx` 使服务器监听特定端口。

低于 1024 的所有端口都需要 `root`权限，因此建议使用高于 1024 的端口。

默认端口为 7860

- 主题

添加 `--theme` 参数来切换主题

## 自定义运行

进一步熟悉这个程序，你会发现可以修改 Bat 运行脚本 添加参数！具体参数列表请读下文。

生成报错？请读 `绘画调试` 章节。

首先，你可以运行 `python webui.py --help` 查看所有命令参数，或者在 [源码文件](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/shared.py) 中读到它们。

Tip:
自定义程序运行方式的推荐方法是编辑 webui-user.bat(Windows) 和 webui-user.sh(Linux)

*环境定制*

`set PYTHON=b:/soft/Python310/Python.exe`

`set PYTHON` 允许设置自定义 Python 路径

`set VENV_DIR=C:\run\var\run`
set VENV_DIR 允许您选择虚拟环境的目录。默认为`venv`。特殊值`-`在不创建虚拟环境的情况下运行脚本。

`set COMMANDLINE_ARGS=--ckpt a.ckpt`
set COMMANDLINE_ARGS 设置命令行参数 webui.py 运行
示例使用模型 a.ckpt 而不是 model.ckpt

*GPU 指定*

选择要使用的默认 Gpu `--device-id 0`，来代替旧版本 (2022/10 之前）的 `CUDA_VISIBLE_DEVICES=0`，可以选择第二个 GPU 允许同时运行两个实例，从而能够以更简洁的方式简单地选择设备。

查看 GPU 型号和 CUDA 版本，可以使用 `nvidia-smi` 命令

**优化命令参数**

来自 [官方 Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Optimizations)

| 命令行参数    | 解释 |
| ----------- | ----------------------------------------- |
| `--xformers`                    | 使用 [xformers](https://github.com/facebookresearch/xformers) 库。极大地改善了内存消耗和速度。Windows 版本安装由 [C43H66N12O12S2 维护](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) 的二进制文件|
| `--force-enable-xformers`       | 无论程序是否认为您可以运行它，都启用 xformers。不要报告你运行它的错误。     |
| `--opt-split-attention`         | Cross attention layer optimization 优化显着减少了内存使用，几乎没有成本（一些报告改进了性能）。黑魔法。默认情况下`torch.cuda`，包括 NVidia 和 AMD 卡。|
| `--disable-opt-split-attention` | 禁用上面的优化|
| `--opt-split-attention-v1`      | 使用上述优化的旧版本，它不会占用大量内存（它将使用更少的 VRAM，但会限制您可以制作的最大图片大小）。|
| `--medvram`                     | 通过将稳定扩散模型分为三部分，使其消耗更少的 VRAM，即 cond（用于将文本转换为数字表示）、first_stage（用于将图片转换为潜在空间并返回）和 unet（用于潜在空间的实际去噪），并使其始终只有一个在 VRAM 中，将其他部分发送到 CPU RAM。降低性能，但只会降低一点-除非启用实时预览。|
| `--lowvram`                     | 对上面更彻底的优化，将 unet 拆分成多个模块，VRAM 中只保留一个模块，破坏性能|
| `*do-not-batch-cond-uncond`     | 防止在采样过程中对正面和负面提示进行批处理，这基本上可以让您以 0.5 批量大小运行，从而节省大量内存。降低性能。不是命令行选项，而是使用`--medvram`or 隐式启用的优化`--lowvram`。   |
| `--always-batch-cond-uncond`    | 禁用上述优化。只有与`--medvram`或`--lowvram`一起使用才有意义 |
| `--opt-channelslast`            | 更改 torch 内存类型，以稳定扩散到最后一个通道，效果没有仔细研究。|

[讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889) 有人认为，通过在 Windows 设置上禁用硬件加速 GPU 调度，WebUi 性能提高了大约 10-50%

## API 文档

使用 `--api` 参数运行程序，在浏览器访问 `{输出的网址}/docs` 就可以查看到 WebUi 的 [Api](https://github.com/AUTOMATIC1111/stable-diffusion-webui/tree/master/modules/api) 文档。[^4]

如果你想让其他应用调用 Api, 追加`--enable-insecure-extension-access`。

下面是一个同步类实现。

```python
import time
import json
import requests
import io
import base64
from PIL import Image, PngImagePlugin

class WebUiApi(object):
    def __init__(url):
        self.url=url

    def txt2img(payload,outpath:str=None,infotie:bool=True):
        payload_json = json.dumps(payload)
        response = requests.post(url=f'{self.url}/sdapi/v1/txt2img', data=payload_json).json()
        # response 响应包含 images、parameters 和 info,image 可能会含有多个图像。
        for i in response['images']:
            # 解码 base64
            image = Image.open(io.BytesIO(base64.b64decode(i)))
            # 元信息输出
            pnginfo = PngImagePlugin.PngInfo()
            if infotie:
               pnginfo.add_text("parameters", str(response['info']))
            # 保存，因为本地不会自动生成文件。
            if not outpath:
               print("Random file name")
               outpath=f"{str(time.time())}.png"
            image.save(outpath, pnginfo=pnginfo)

payload = {
    "prompt": "1girl",
    "steps": 20
}
# 其他参数会使用默认值
WebUiApi(url="http://127.0.0.1:7860").txt2img(payload=payload,outpath="1145.png",infotie=True)

# 实际使用的时候不应该保存到本地再发送，而是直接发送，避免存储图片作品造成 版权 问题。
```

跨平台多后端项目 [novelai-bot](https://github.com/koishijs/novelai-bot)

Discord 机器人项目 [aiyabot](https://github.com/Kilvoctu/aiyabot/blob/main/core/stablecog.py)

[^3]:[Wiki2](https://rentry.co/voldy)

[^4]:[Basic Documentation and Examples for using API](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3734)
