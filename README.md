

## Retrieval-Augmented Machine Translation with Unstructured Knowledge
This repository contains the resources for our paper ["Retrieval-Augmented Machine Translation with Unstructured Knowledge"](https://arxiv.org/abs/2412.04342)

#### Updates
- 🌟 *2025.09.04*: We release all data of RAGTrans, including 79K English-to-Chinese and 90K English-to-German translation samples. You can download our data from [this link](https://drive.google.com/file/d/1VzajoI2bUO-Tk17bmwQkzA-5kO9mBeyx/view?usp=sharing).
- 🌟 *2025.08.21*: Our work is accepted to **EMNLP 2025 Findings**.
- ~~🌟 *2024.12.24*: We release the validation and the testing sets of RAGtrans. Please refer to `data/` folder.~~


If you find this work is useful, please consider cite our paper:

```
@article{wang2024retrieval,
  title={Retrieval-Augmented Machine Translation with Unstructured Knowledge},
  author={Wang, Jiaan and Meng, Fandong and Zhang, Yingxue and Zhou, Jie},
  journal={arXiv preprint arXiv:2412.04342},
  year={2024}
}
```

### 1. Overview

In this work, we bring the idea of RAG into machine translation (MT), and study how to enhanced MT models with retrieved unstructured knowledge (documents). To this end, we introduce:

- **One benchmark dataset**: We propose RAGtrans dataset that contains 79K MT samples, each of which consists a source English sentence, a (relevant or noisy) document (in English, Chinese, German, French or Czech) and the corresponding Chinese translation (obtained via GPT-4o or professional human translators).
- **One training framework**: We propose CSC multi-task training method with three designed objectives to enhance models' retrieval-augmented MT ability.


### 2. RAGtrans

You can download our data from [this link](https://drive.google.com/file/d/1VzajoI2bUO-Tk17bmwQkzA-5kO9mBeyx/view?usp=sharing).

#### Data Format

For the training and the validation samples, the format is as follows:
```json
{
    "src": "He was initially a member of the Liberal Democratic Party, serving as its secretary general from 1989 to 1991. He left the LDP in 1993 and subsequently served as head of a number of other political parties, first by co-founding the Japan Renewal Party with Tsutomu Hata, which formed a short-lived coalition government with several other parties opposed to the LDP. Ozawa later served as president of the opposition New Frontier Party from 1995 to 1997, president of the Liberal Party from 1998 to 2003, president of the opposition Democratic Party of Japan from 2006 to 2009 and secretary-general of the DPJ in government from 2009 to 2010.",
    "tgt": "他最初是自由民主党的成员，从1989年到1991年担任该党的干事长。他于1993年离开自民党，随后与羽田孜共同创立了日本新党，并与其他几个反对自民党的政党组成了一个短暂的联合政府。小泽后来在1995年至1997年担任反对党新进党的党首，1998年至2003年担任自由党的党首，2006年至2009年担任反对党日本民主党的党首，并在2009年至2010年担任执政的民主党的干事长。",
    "doc": "Ozawa was born in Tokyo on 24 May 1942. His father, Saeki, was a self-made businessman, who was elected to the House of Representatives from Iwate district. The hometown of his family was Mizusawa, Iwate, which remained the stories of the Emishi leader Aterui's resistance movement.  Ozawa attended Keio University, graduating in 1967, and entered postgraduate school in Nihon University. Ozawa was majoring in law and intended to become an attorney. In May 1968, his father died of heart failure.",
    "tag": "clean_en"
}
```
where "src" and "tgt" indicate the source English sentence and the target Chinese/German sentence, respectively. "doc" indicates the corresponding document w.r.t the <src, tgt> pair, the document could be relevant doc or noisy doc.

"tag" represents the type of the document:
- `clean_en/zh/de/fr/cs` means the English/Chinese/German/French/Czech document is relevant to current <src, tgt> pair.
- `noisy_en/zh/de/fr/cs` means the English/Chinese/German/French/Czech document is irrelevant to current <src, tgt> pair.


For the testing samples, the format is as follows:
```json
{
    "src": "The King’s Highway was a trade route of vital importance in the ancient Near East, connecting Africa with Mesopotamia. It ran from Egypt across the Sinai Peninsula to Aqaba, then turned northward across Transjordan, to Damascus and the Euphrates River.",
    "tgt": "君王大道是古代近东地区的重要贸易通道，它将非洲与美索不达米亚连接起来。这条路从埃及出发，穿过西奈半岛到达亚喀巴，随后向北经过外约旦，最终抵达大马士革和幼发拉底河。",
    "en_doc": "【King's Highway (ancient)】The Highway began in Heliopolis, Egypt and then went eastward to Clysma (modern Suez), through the Mitla Pass and the Egyptian forts of Nekhl and Themed in the Sinai desert to Eilat and Aqaba. From there the Highway turned northward through the Arabah, past Petra and Ma'an to Udhruh, Sela, and Shaubak. It passed through Kerak and the land of Moab to Madaba, Rabbah Ammon/Philadelphia (modern Amman), Gerasa, Bosra, Damascus, and Tadmor, ending at Resafa on the upper Euphrates.",
    "zh_doc": "【君王大道】君王大道开始于埃及的赫里奥波里斯,向东抵达克里斯马(今天的苏伊士),经由米特拉山口与古埃及在西奈沙漠的要塞城市尼赫堡垒而抵达今天的以色列国土埃拉特和约旦境内的亚喀巴。从那儿大道转向穿越阿拉伯谷,经过佩特拉、马安抵达西拉和蒙特利尔 (十字军城堡)。经由卡拉克和摩押人的土地前往米底巴、抵达拉巴安盟/费拉德尔菲亚(今天的安曼)、杰拉什,进入今叙利亚境内的布斯拉、大马士革与泰德穆尔,最终结束于幼发拉底河上游的雷萨法。",
    "de_doc": "【Königsstraße (Jordanien)】Die Mescha-Stele erwähnt den Ausbau der Königsstraße als eine der Leistungen des Königs von Moab: „Ich baute Aroër, ich schuf die Straße am Arnon.“ Die Königsstraße war eine Lebensader des Moabiterreichs. „Vermutlich ist dieser Zeit eine Karawanserei zuzuordnen, die bei Khirbet Arair freigelegt wurde und 50 × 50 Meter misst.“ Die Befestigung gerade dieses Straßenabschnitts war strategisch wichtig, denn das schroff abfallende Wadi al-Mujib bildete eine natürliche Grenze. Das Kernland von Moab befand sich südlich des Wadi, aber die Könige von Moab beherrschten lange Zeit hindurch auch Gebiete weiter nördlich bis nach Madaba. Zwei Kastelle an diesem Punkt stammen aus nabatäischer und römischer Zeit.  Der Name „Königsstraße“ wird in der Bibel (4. Buch Mose 20, 17 und 21, 22) nicht erklärt, sondern als allgemein bekannt vorausgesetzt. Den Israeliten wird bei ihrem Weg ins Gelobte Land die Benutzung der Königsstraße von den Edomitern und Amoritern verweigert. Hier erfährt man, dass die Straße durch fruchtbares Land führte: es gab Felder, Weinberge und Brunnen. Das zeichnete sie gegenüber einer etwa 30 km weiter östlich verlaufenden parallelen Route durch die Wüste aus, welche kürzer, aber gefährlicher war.  Herodot erwähnt eine Königsstraße als Verbindung zwischen dem persischen Reich und dem Mittelmeer; die Karawanenstraße durch das heutige Jordanien war ein südlicher Zweig dieser großen Handelsroute.",
    "noisy_doc": "The gameplay of \"Papers, Please\" focuses on the work life of an immigration inspector at a border checkpoint for the fictional country of Arstotzka in the year 1982. At the time frame of the game, Arstotzka has recently ended a six-year long war with the neighboring country of Kolechia yet political tensions between them and other nearby countries remain high. As the checkpoint inspector, the player reviews arrivals' documents and uses an array of tools to determine whether the papers are in order for the purpose of arresting certain individuals such as terrorists, wanted criminals, smugglers and entrants with forged or stolen documents; keeping other undesired individuals like those with no polio vaccine including anti-vaxxers, expired vaccines, missing required paperwork or expired paperwork out of the country; and allowing the rest through. For each in-game day, the player is given specific rules on what documentation is required and conditions to allow or deny entry which become progressively more complex as each day passes. One by one, immigrants arrive at the checkpoint and provide their paperwork. The player can use a number of tools to review the paperwork to make sure it is in order. When discrepancies are discovered, the player may interrogate the applicant, demand missing documents, take the applicant's fingerprints while simultaneously ordering a copy of the applicant's identity record in order to prove or clear either name or physical description discrepancies, order a full body scan in order to clear or prove weight or apparent biological sex discrepancies or find enough incriminating evidence required to arrest the entrant. There are opportunities for the player to have the applicant detained and the applicant may, at times, attempt to bribe the inspector. The player ultimately must stamp the entrant's passport (or temporary visa slip if the individual has no passport) to accept or deny entry unless the player orders the arrest of the entrant. If the player has violated the protocol, a citation will be issued to the player shortly after the entrant leaves. Generally the player can make two violations without penalty, but subsequent violations will cost the player increasing monetary penalties from their day's salaries. The player has a limited amount of real time, representing a full day shift at the checkpoint, to process as many arrivals as possible. At the end of each in-game day, the player earns money based on how many people have been processed (5 credits for each individual that enters the booth before the shift ends) and bribes collected, less any penalties for protocol violations, and then must decide on a simple budget to spend that money on rent, food, heat and other necessities in low-class housing for themselves and their family. The player must also make certain not to earn too much money in illegitimate ways, lest his family be reported and have all the money they had accumulated thus far confiscated by the government."
}
```
where `src` and `tgt` indicate the source English sentence and the target Chinese sentence, respectively. `en_doc`, `zh_doc` and `de_doc` represent the corresponding relevant English, Chinese and German documents, respectively. `noisy_doc` denotes an irrelevant document, which is used in noisy setting in our experiments (i.e., w/ Noisy Document).

#### Model Training

Before model training, we organize the training data using the following templates:
- English-to-Chinese template:
```json
{
    "system": "You are a professional translator, and your task is to translate an given input sentence from English to Chinese. In addition to the input sentence, you will be provided with a document that may contain relevant information to aid in the translation. However, be aware that some documents may contain irrelevant or noisy information.",
    "instruction": "<document>\n[document]\n</document>\n<input sentence>\n[source sentence]\n</input sentence>",
    "output": "[translation]"
}
```
where `[document]`, `[source sentence]` and `[translation]` denote the involved document, the source sentence and the target translation, respectively.

- English-to-German template:
```json
{
    "system": "You are a professional translator, and your task is to translate an given input sentence from English to German. In addition to the input sentence, you will be provided with a document that may contain relevant information to aid in the translation. However, be aware that some documents may contain irrelevant or noisy information.",
    "instruction": "<document>\n[document]\n</document>\n<input sentence>\n[source sentence]\n</input sentence>",
    "output": "[translation]"
}
```

For model training (SFT), we use the [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory) framework, the training script is:
```bash
deepspeed --include localhost:0,1,2,3,4,5,6,7 --master_port 2556 [llamafactory_path]/src/train.py \
    --stage sft \
    --model_name_or_path [Qwen2.5-7B-Instruct_path] \
    --dataset_dir [data_path] \
    --do_train \
    --template qwen \
    --dataset [dataset_tag] \
    --finetuning_type full \
    --output_dir [save_path] \
    --overwrite_cache \
    --per_device_train_batch_size 1 \
    --gradient_accumulation_steps 4 \
    --preprocessing_num_workers 32 \
    --lr_scheduler_type cosine \
    --logging_steps 5 \
    --save_strategy epoch \
    --cutoff_len 2000 \
    --packing true \
    --use_fast_tokenizer false \
    --learning_rate 1e-5 \
    --max_grad_norm 0.5 \
    --num_train_epochs 1.0 \
    --plot_loss \
    --bf16 \
    --flash_attn fa2 \
    --upcast_layernorm \
    --deepspeed [deepseed_config_file_path]
```
where `[llamafactory_path]` denotes the path of llamafactory codes. `[Qwen2.5-7B-Instruct_path]` denotes the local path of the backbone LLM, and here we use [Qwen2.5-7B-Instruct](https://huggingface.co/Qwen/Qwen2.5-7B-Instruct) as an example.

For `[data_path]` and `[dataset_tag]`, please refer [here](https://github.com/hiyouga/LLaMA-Factory/tree/main/data) for more details.  

For `[deepseed_config_file_path]`, you can use the following setting to reproduce our work:
```json
{
    "train_batch_size": "auto",
    "train_micro_batch_size_per_gpu": "auto",
    "gradient_accumulation_steps": 4,
    "zero_allow_untested_optimizer": true,
    "bf16": {
      "enabled": true
    },
    "zero_optimization": {
      "stage": 2
    }
}
```

<!-- 
<hr>

✨✨✨Merry Chrismas✨✨✨
<p>
    <br>
    <img src="./figs/UBR_20241124.jpg" height="600"/>
    <br>
</p>
Universal Studios, Beijing, Nov. 24, 2024 -->