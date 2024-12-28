# finetune a Chinese hubert model - preprocess stage

> 本笔记用于修订师兄用于微调中文hubert模型的预处理过程
>
> 记于 2024.12.28 实验室年会前

### Version

- os centos6
- Python 3.9.2
- gcc 9.3.0
- cuda 11.8
- fairseq v0.12.2

### 数据处理

> TODO 打算写一篇笔记从头记录 asr 从数据处理到LM模型输出的过程

对于fairseq hubert finetune， 我们需要3个文件，分别是 tsv 记录音频文件地址，ltr 记录文本标签，dict 文件记录文本标签中各个字符的使用情况。

- finetune

  - data

    - train.tsv
    - valid.tsv
  - trans

    - dict.ltr.txt
    - train.ltr
    - valid.ltr

    

> TODO 所有的处理脚本

**tsv**

tsv 第一行记录音频文件的根目录，接下来所有行数记录音频文件名与音频帧数(**中间用"\t"间隔，而不是空格**)

```
/path/to/base
/name.wav	1314
...
```

一个trick是第一行用/表示，接下来音频保存其绝对路径(比较直观)

```
/
/solute/path/to/your/name.wav	1314
...
```

**ltr**

ltr 与 tsv 中音频的**顺序一一对应**，形式如下所示：

```
然 | 后 | 像 | 我 | 当 | 初 | 像 | 我 | 给 | 感 | 觉 | 有 | 点 | 贵 |
```

请注意，文本标签已经被分成了token的形式，并且单独的token之间**有空格和|表示**

**dict**

dict的形式如下所示

```
| 94802
的 1314
我 1314
...
```

> |会以绝对优势在第一位，很直观

可以用fairseq中自带的脚本处理，**生成的dict文件名可能和需要的不对齐，注意修改**

```shell
python preprocess.py --task audio_finetuning \
    --trainpref /path/to/your/train.ltr \
    --validpref /path/to/your/valid.ltr \
    --destdir /path/to/your/output_dir --dict-only
```