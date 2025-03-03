# add a timestep for your speech

> 如何添加一个字级别时间戳
>
> 中文用paraformer，这是一个很简单的一选一

```
from funasr import AutoModel
local_path_root="/Work21/2024/lixuanchen/experiment/FunASR/examples/industrial_data_pretraining/paraformer/modelscope_models"
from modelscope import snapshot_download

model = AutoModel(model=f"{local_path_root}/speech_timestamp_prediction-v1-16k-offline")

wav_file = "/Work21/2024/lixuanchen/project/SpeechWellness/data/SW1-dev/0524/0524-0.wav"
text_file = "有这个肯定有的。然后我一般就是画画一个人呆着画，就是随便涂颜色或者调颜色，或者是写小说之类的写字，就是一直写写满一个本子，或者是画画，就是找一张喜欢的人物，或者动漫人物照的话，或者是调颜色，拿那个拿那个就弄送水的那种东西，然后就在里面调那个颜色，然后再把它们全部印在我的纸上面去等么干。然后我就特别喜欢这样子，调节我的情绪一般是没有人比较好，有人的话一般都不会那么干，或者是看会儿手机听音乐、看小说职位的。"
# wav_file = f"{model.model_path}/example/asr_example.wav"
# text_file = f"{model.model_path}/example/text.txt"

res = model.generate(input=(wav_file, text_file), data_type=("sound", "text"))
print(res)
```

以下是结果

> rtf_avg: 0.103: 100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:00<00:00,  2.16it/s]
> [{'key': 'rand_key_2yW4Acq9GFz6Y', 'text': '欢 迎 大 家 来 到 魔 搭 社 区 进 行 体 验', 'timestamp': [[1190, 1410], [1410, 1610], [1610, 1830], [1830, 2010], [2010, 2230], [2230, 2430], [2430, 2650], [2650, 2890], [2890, 3130], [3130, 3370], [3410, 3650], [3690, 3930], [3950, 4190], [4230, 4395]]}]

speech_timestamp_prediction-v1-16k-offline 这一模型可以在魔搭上找到