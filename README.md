

## Retrieval-Augmented Machine Translation with Unstructured Knowledge
This repository contains the resources for our paper ["Retrieval-Augmented Machine Translation with Unstructured Knowledge"](https://arxiv.org/abs/2412.04342)

#### Updates
- 🌟 *2024.12.24*: We release the validation and the testing sets of RAGtrans. Please refer to `data/` folder.


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
where "src" and "tgt" indicate the source English sentence and the target Chinese sentence, respectively. "doc" indicates the corresponding document w.r.t the <src, tgt> pair, the document could be relevant doc or noisy doc.

"tag" represents the type of the document:
- `clean_en/zh/de/fr/cs` means the English/Chinese/German/French/Czech document is relevant to current <src, tgt> pair.
- `noisy_en/zh/de/fr/cs` means the English/Chinese/German/French/Czech document is irrelevant to current <src, tgt> pair.


For the testing samples, the format is as follows:
```json
{
    "src": "Urban search and rescue is a type of technical rescue operation that involves the location, extrication, and initial medical stabilization of victims trapped in an urban area, namely structural collapse due to natural disasters, mines and collapsed trenches.",
    "tgt": "坍塌搜救专队是一种技术救援操作类型，涉及在城市区域定位、解救被困人员，以及对被困人员，尤其是因自然灾害导致的建筑物坍塌、矿井和塌陷沟渠所致进行初步的医疗稳定处理。",
    "en_doc": "USAR task forces are often categorized for standardization. Depending upon the classification, there may be close to 70 positions. To be sure a full team can respond to an emergency, USAR task forces have at the ready more than 140 highly trained members. A task force is often a partnership between local fire departments, law enforcement agencies, federal and local governmental agencies and private companies. In the United States, these can be federally endorsed teams or state teams activated through mutual aid agreements. In England, the responsibility for USAR lies with local authority fire and rescue services. Equipment supplied to them is part of a government initiative known as the New Dimension programme, which provides the training and equipment.  USAR teams bring together, in an integrated response: highly trained personnel from the emergency services along with engineers, medics and search dog pairs, specialised equipment effective communications established methods of command and control logistical support procedures to request international assistance if required under an international search and rescue framework. The training that teams receive is an ongoing procedure combining classes from the local fire and rescue services and government agencies.",
    "zh_doc": "【坍塌搜救专队】坍塌搜救专队的人员从特种救援队中挑选组成,人员日常被编入红、黄及蓝3队,分别驻守于香港总区西湾河消防局、九龙总区启德消防局及新界总区东涌消防局;每局每日至少有8名人员当值,以每3个月轮更制执勤,日班及夜班分别有7辆及3辆救护人员当值的救护车候命。另外,坍塌搜救专队辖下有搜索犬专队。  人员轮流候命准备随时执行任务,每次候命总期为3月;平常则执行日常岗位的职务。一旦发生重大坍塌事故,就会奉命出动。因应需要,坍塌搜救专队亦会出勤出境协助境外事故处理。",
    "de_doc": "【Urban Search and Rescue】In Deutschland, Österreich und der Schweiz gibt es mehrere USAR-Teams, die ins Ausland geschickt werden können. In Deutschland die SEEBA (Schnelleinsatzeinheit für Bergung im Ausland der Bundesanstalt Technisches Hilfswerk), die I.S.A.R. Germany (International Search and Rescue), @fire Internationaler Katastrophenschutz Deutschland e.V., den Bundesverband Rettungshunde (BRH) und die Deutsche Erdbebenrettung – Abt. U.S.A.R. In Österreich die AFDRU und das SA-RRT MUSAR Modul des Arbeiter-Samariter-Bund Österreichs und in der Schweiz die Rettungskette Schweiz.  Teilweise sind diese Einheiten nach der Zertifizierung Mitglieder in der International Search and Rescue Advisory Group (INSARAG). Diese legt Standards über die Größe der Teams und die Arbeitsweisen fest. Alle USAR-Teams, unabhängig ihrer Klassifizierung und operativen Beteiligung, sollten aus den Komponenten Management, Logistik, Search, Rescue und Medical bestehen. Das INSARAG-USAR-Team-Klassifizierungssystem weist drei Level der Klassifizierung auf: Light, Medium und Heavy USAR Teams.  \"Light USAR Teams\" haben die operative Fähigkeit, sofort nach Eintreten der Katastrophe oberflächliche \"search and rescue\" Arbeiten durchzuführen. Gewöhnlicherweise kommen Light USAR Teams aus dem betroffenen Land oder aus den Nachbarstaaten. Es ist nicht vorgesehen, dass Light USAR Teams zu internationalen Hilfseinsätzen herangezogen werden.  \"Medium USAR Teams\" haben die operative Fähigkeit in zerstörten Strukturen technische \"search and rescue\" Arbeiten durchzuführen. Medium USAR Teams sind fähig, Beton zu brechen, zu durchbrechen und zu schneiden. Von Medium USAR-Teams wird nicht erwartet, Stahlbeton zu brechen, zu durchbrechen oder zu schneiden. Internationale Medium USAR Teams reisen in ein betroffenes Land innerhalb von 32 Stunden, nachdem dies im virtuellen On-Site Operations Coordination Centre (OSOCC) verlautbart wurde.  \"Heavy USAR Teams\" haben die operative Fähigkeit in zerstörten Strukturen, die auch mit Stahl und Stahlbeton verstärkt sind, schwierige technische \"search and rescue\" Arbeiten durchzuführen. Heavy Teams sind auf den internationalen Einsatz nach Katastrophen, bei denen sehr viele zerstörte Strukturen zu finden sind, ausgerichtet und kommen zum Einsatz, wenn die nationalen Ressourcen nicht ausreichen oder erschöpft sind. Internationale Heavy USAR Teams reisen in ein betroffenes Land innerhalb von 48 Stunden, nachdem dies in der virtuellen OSOCC verlautbart wurde.  In Deutschland wurde am 2. September 2007 die SEEBA der BA THW in den Klassen „Medium USAR“ und „Heavy USAR“ bei einer Klassifizierungsübung (IEC) entsprechend den Vorgaben der INSARAG zertifiziert. Am 4. Oktober 2007 wurde I.S.A.R. Germany als „Medium USAR“ zertifiziert."
}
```
where "src" and "tgt" indicate the source English sentence and the target Chinese sentence, respectively. "en_doc", "zh_doc" and "de_doc" represent the corresponding relevant English, Chinese and German documents, respectively.


<hr>

✨✨✨Merry Chrismas✨✨✨
<p>
    <br>
    <img src="./figs/UBR_20241124.jpg" height="600"/>
    <br>
</p>
Universal Studios, Beijing, Nov. 24, 2024