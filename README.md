# batchOptimize

## 备忘录

### Markdown介绍

该Readme文件由Markdown格式编写，这种语言的设计目标是可读性和易用性。其核心理念是，一份 Markdown 格式的文档应该能够直接以纯文本的形式阅读，而无需查看其渲染后的效果。

Markdown主要语法规则：https://www.jianshu.com/p/191d1e21f7ed/

在VSCode中直接点开Readme文件编辑即可。

### FastAPI框架下如何返回PIL格式的图片？

如果你希望使用FastAPI返回Python Imaging Library (PIL)格式的图片，你需要先将PIL Image对象转化为一个字节流（例如，将其保存为JPEG或PNG格式的数据），然后你可以使用`Response`对象将这个字节流返回给客户端。

```python
from fastapi import FastAPI
from fastapi.responses import Response
from PIL import Image
import io

app = FastAPI()

@app.get("/image/")
def read_image():
    image = Image.new('RGBA', size=(50, 50), color=(155, 0, 0))
    byte_arr = io.BytesIO()
    image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return Response(content=byte_arr, media_type='image/png')

```

这个示例创建了一个新的PIL Image对象，并将其保存为PNG格式的数据到一个`BytesIO`对象中。然后这个`BytesIO`对象被转化为字节，并用作FastAPI `Response`对象的内容。返回的媒体类型设置为'image/png'，以通知客户端响应内容的格式。

这个示例中的图像是红色的50x50像素大小的图片。你可以根据你的需求修改此示例，比如使用已经存在的PIL Image对象。

### 如何规范使用git commit

在团队进行 Git 合作开发项目时，规定 Git commit 的时机是非常重要的，这有助于维护代码的清晰性和可读性，同时也能避免出现冲突。以下是一些常见的实践和建议：

1. **分阶段提交：** 一般来说，每次完成一项功能或修复一个 bug 时，就应该进行一次 commit。避免长时间不提交或多个功能混在一起提交，这样可以使每个 commit 都有明确的目标，同时也方便代码审查和出现问题时回滚。
2. **单一职责原则：** 每一个 commit 都应该只做一件事情。如果一个 commit 中包含多个改动（例如修复了一个 bug 并添加了一个新功能），这会使得版本历史难以理解，也不利于后续的代码审查和问题追踪。
3. **编写清晰的 commit 信息：** 提交时，编写一个清晰明了的 commit 信息是非常重要的。这个信息应该准确反映这个 commit 所做的改动。使用 imperative mood（命令语气）的开头动词，例如 "Add"、"Fix"、"Change"、"Remove" 等，可以让 commit 信息更加规范。
4. **测试前提交：** 在进行单元测试或集成测试之前，进行一次 commit 可以为可能出现的问题提供一个回滚点。
5. **合并提交：** 如果一系列的小改动都是为了完成一个大的功能，可以使用 git rebase 或 git merge --squash 来将这些改动合并为一个 commit。这样可以保持版本历史的整洁，但也要注意这可能会丢失一些中间的开发历史。
6. **频繁提交，定期推送：** 鼓励开发者频繁地进行 commit，但不需要每次 commit 都进行 push。可以在一天的工作结束时，或者完成了一项重要的工作时，将本地的改动 push 到远程仓库。

以上的建议可能需要根据你的团队具体情况进行调整。一些团队可能会有自己的规范，例如使用特定的 commit 信息格式，或者要求每个 commit 都必须通过 CI 测试。总的来说，重要的是确保每个 commit 都是有意义的，可以独立理解，且能准确反映出改动的内容。

### .gitignore的使用方法

`.gitignore` 是一个文本文件，用于告诉 Git 哪些文件或者文件夹不需要添加到版本控制中。这可以帮助你排除某些不需要跟踪的文件，比如编译后的二进制文件、日志文件、用户设置等。

以下是 `.gitignore` 的使用方法：

1. **创建 .gitignore 文件：** 在你的 Git 项目根目录下创建一个名为 `.gitignore` 的文件。在这个文件中，你可以列出所有你希望 Git 忽略的文件和文件夹。

2. **指定忽略的文件或文件夹：** 在 `.gitignore` 文件中，每一行都代表一个忽略的模式。例如：

   ```bash
   # 忽略所有的 .a 文件
   *.a
   
   # 但跟踪所有的 lib.a，即使你在前面忽略了 .a 文件
   !lib.a
   
   # 只忽略当前目录下的 TODO 文件，不忽略 subdir/TODO
   /TODO
   
   # 忽略所有在 build/ 目录下的文件
   build/
   
   # 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
   doc/*.txt
   
   # 忽略所有的 .pdf 文件在 doc/ 目录下的和子目录
   doc/**/*.pdf
   
   ```

3. ​	**特殊字符：** `.gitignore` 文件支持使用一些特殊字符，如下所示：

   - `*` 匹配任意数量的任意字符
   - `?` 匹配任意单个字符
   - `**` 匹配任意数量的任意目录，当它跟在 / 后面时
   - `/` 表示目录
   - `#` 表示注释
   - `!` 表示否定，即不忽略匹配该模式的文件或目录

注意：`.gitignore` 文件只能影响那些还没有被 Git 追踪的文件。如果你在添加 `.gitignore` 文件之前就已经把文件添加到版本控制中，那么这个文件的改动依然会被 Git 追踪。要解决这个问题，你需要先用 `git rm --cached` 命令把文件从版本控制中移除。

## Serverless Function Cost

本文对国内外云服务商Serverless Functions计价进行调研，仅用作学术研究，实际定价请自行访问文档。

### Ali Cloud

阿里云Serverless计价事例（不考虑公网流量）:

![AliCloud](README.assets/AliCloud.png)

$$
C_{Ali}=I\times T\times (N_{CPU}\times P_{CPU}+M\times P_{Mem}+N_{GPU}\times P_{GPU})+I\times P_{req}
$$

其中$I$为调用次数，T为函数持续时间，$N_{CPU}$， $M$, $N_{GPU}$分别为CPU核数，1024MB单位内存数和1024MB单位显存数，$P_{CPU}$, $P_{Mem}$ 和 $P_{GPU}$为单位CPU，内存，GPU价格,$P_{req}$为单次函数调用价格，此公式为函数触发时的价格。

![截屏2023-05-24 21.41.01](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2021.41.01.png)

如果实例在一段时间内（一般为3~5分钟）不处理请求，会自动销毁。首次发起调用时，需要等待实例冷启动。如果预热（Keep alive）的话GPU$(P_{GPU})$和CPU ($P_{CPU})$分别为活跃时的$10\%$，内存单价$P_{Mem}$则不变。

![fc-billing](README.assets/p558376.png)

### Tencent Cloud

与Ali云类似，不过在部署函数时只能调整内存资源。

![截屏2023-05-24 21.18.45](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2021.18.45.png)

$$
C_{Tencent} = I\times T\times M \times P_{Mem} + I\times P_{req} + C\times D
$$

其中$I$为调用次数，T为函数持续时间，M为分配内存数量，$P_{req}$ 为单次调用单价，$C$为每日门槛价格，$D$为使用天数。

下面一个案例给出了闲置资源价格为 0.00005471元/GBs，仅算内存占用价格。

![截屏2023-05-24 22.00.28](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2022.00.28.png)

### Huawei Cloud

华为云采用阶梯计费方式，只能分配内存，其余计价方式与腾讯云类似，不清楚闲置成本。

![截屏2023-05-24 21.50.57](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2021.50.57.png)

1. 华为云函数工作流FunctionGraph按照实际使用量付费，没有最低消费。计费模式采用累计分档计费，请求次数和计量时间均按月累计使用量，按天扣费。
2. 总费用 = 请求次数费用 + 计量时间费用。
3. 未使用预留实例的情况下，函数计算资源消耗是函数所选内存和函数执行时间的乘积，执行时间是从函数代码开始执行的时间算起到其返回或终止的时间为止，计量的粒度是1毫秒，不足1毫秒按1毫秒计费，例如函数执行了0.5毫秒，会按照1毫秒计费。例如函数内存规格为512MB，函数执行1秒的计算资源消耗为：0.5GB*1秒= 0.5GB-秒，其他内存规格依此类推。
4. 使用预留实例的情况下，资源消耗计算稍有不同，FunctionGraph根据预留实例设置在实例创建成功后开始计时。如果预留实例存活时间不足1分钟，将按照 1 分钟计算，超过一分钟的部分，按照秒的粒度向上取整计算。例如一个512M的函数预留实例运行时间为 51 秒，将按照1分钟计算，计算资源消耗为 0.5G * 60s = 30GB-秒。运行时间为 60.5 秒，则计费时间为 61 秒，计算资源消耗为0.5G * 61s = 30.5GB-秒。
5. 计费时，FunctionGraph将累加各个函数的计算资源消耗。例如当前账号下有两个函数A和B，A函数累计请求次数为200万次，计量时间为200,000GB-秒，B函数请求次数为200万次，计量时间为300,000GB-秒，那么请求次数费用为：￥1.33 元/100万次 *（ 200万次 + 200万次 - 100万次） = ￥3.99元， 计量时间费用为：￥0.00011108 元/GB-秒 *（ 200,000GB-秒 + 300,000GB-秒 - 400,000GB-秒） = ￥11.108元，总费用为：￥3.99元 + ￥11.108元 = ￥15.098元。

### AWS Lambda

AWS Lambda 服务计价估计网站：https://calculator.aws/#/estimate

![截屏2023-05-24 21.05.51](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2021.05.51.png)

Lambda计算公式：

$$
C_{\text {Lambda }}=(S \cdot M \cdot I) \cdot K_1+I \cdot K_2,
$$

where S is the length of the function call (referred as batch service time here), M is the memory allocated for the function, I is the number of calls to the function that decreases when requests are batched together, K1 (i.e., $1.66667 · 10^{−5} $/GB-s) is the cost of the memory, and K2 (i.e., $2 · 10^{−7} $) is the cost of each call to the function.

预配置并发定价（即keep alive，闲置定价）：

![截屏2023-05-24 22.04.10](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2022.04.10.png)

### Google Cloud Platform (GCP)

GCP函数资源分配较为单一，只能从7种配置中选择。

![截屏2023-05-24 21.24.33](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2021.24.33.png)

![截屏2023-05-24 22.05.00](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2022.05.00.png)

简单来说GCP定价与阿里云基本一致，详情事例：

一个预配了 128 MB 内存和一个 200 MHz CPU 的简单事件驱动函数，每个月调用 1 千万次，每次运行 300 毫秒，并且仅使用 Google API（无计费出站流量）。调用1000万次，则时间为：

1. (128 MB/1024 MB/GB) x 0.3 秒 = 每次调用 0.0375 GB 秒
2. (200 MHz/1000 MHz/GHz) x 0.3 秒 = 每次调用 0.0600 GHz 秒
3. 10000000 次调用 x 0.0375 GB 秒 = 每月 375000 GB 秒
4. 10000000 次调用 x 0.0600 GHz 秒 = 每月 600000 GHz 秒

![截屏2023-05-24 22.07.31](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2022.07.31.png)

### Azure Function

整体同AWS

![截屏2023-05-24 22.09.53](README.assets/%E6%88%AA%E5%B1%8F2023-05-24%2022.09.53.png)
