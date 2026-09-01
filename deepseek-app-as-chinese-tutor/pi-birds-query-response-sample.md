## 1. Prompt Repetition & Chinese Essence

**Original Prompt (English):**  
> You are a Chinese ornithologist. Here’s a list of birds: 「 苍鹭,火尾太阳鸟,白鷳,灰鹡鸰,黑水鷄,翠鸟,白鹳,>白額燕鷗,黑枕燕鷗,粉紅燕鷗,大鳳頭燕鷗,燕鷗,鸊鷉,钳嘴鹳,白鹭,>紅梅花雀,紅嘴藍雀,黑鹳,中华秋沙鸭,青头潜鸭,彩鹦,白鹤,紅尾水鴝,白胸翡翠鳥,海南藍仙鶲,荷花翠鸟,大鶚」. Add pinyin to each name, the common name in English and scientific name; preserve the form (繁體 or 简体) of the name. With this generated list, group the birds by taxonomy, sorting each taxonomy’s birds by the unicode of the bird’s Chinese name. Then return the sorted list to me.

**Chinese Essence (简明口语化):**  
> 你扮演一位中国鸟类学家，给你一份包含 27 种鸟类的清单（有些是繁体，有些是简体）。你要做三件事：给每个鸟名加拼音、英文名和拉丁学名，且**保留原字（繁简不变）**；然后按传统分类（目级）分组；最后在每个组内按中文名的 Unicode 码位从小到大排序。把整理好的清单返回给我。

---

## 2. Bilingual Response

### 中文部分 (Chinese Section)

**摘要**  
已按您的要求整理完毕。共 27 种鸟类，按 **11 个目** 分组，每组内部严格按中文名的 **Unicode 码位（从低到高）** 排序。所有名称均保留您提供的原始字形（繁/简）。清单包含拼音、英文名和学名。对于非标准名“荷花翠鸟”，我根据形态和分类将其鉴定为 **蓝翡翠（*Halcyon pileata*）**，并已在列表中注明。

**详细说明**

**排序逻辑**：每组内比较中文名的完整字符串（按 Unicode 码位），从第一个字符开始依次比较。例如“白”(U+767D) 排在“苍”(U+82CD) 之前；“大”(U+5927) 排在“燕”(U+71D5) 之前。整理结果如下（组内按此规则升序排列）：

---

**1. 雁形目 (Anseriformes)**  
- **中华秋沙鸭** (zhōng huá qiū shā yā) | Scaly-sided Merganser | *Mergus squamatus*  
- **青头潜鸭** (qīng tóu qián yā) | Baer's Pochard | *Aythya baeri*

**2. 䴙䴘目 (Podicipediformes)**  
- **鸊鷉** (pì tī) | Little Grebe | *Tachybaptus ruficollis*

**3. 鹤形目 (Gruiformes)**  
- **白鹤** (bái hè) | Siberian Crane | *Leucogeranus leucogeranus*  
- **黑水鷄** (hēi shuǐ jī) | Common Moorhen | *Gallinula chloropus*

**4. 鸻形目 (Charadriiformes)**（按首字 Unicode：大 U+5927 → 燕 U+71D5 → 白 U+767D → 粉 U+7C89 → 黑 U+9ED1）  
- **大鳳頭燕鷗** (dà fèng tóu yàn ōu) | Great Crested Tern | *Thalasseus bergii*  
- **燕鷗** (yàn ōu) | Common Tern | *Sterna hirundo*  
- **白額燕鷗** (bái é yàn ōu) | Little Tern | *Sternula albifrons*  
- **粉紅燕鷗** (fěn hóng yàn ōu) | Roseate Tern | *Sterna dougallii*  
- **黑枕燕鷗** (hēi zhěn yàn ōu) | Black-naped Tern | *Sterna sumatrana*

**5. 鹳形目 (Ciconiiformes)**（首字：白 U+767D → 钳 U+94B3 → 黑 U+9ED1）  
- **白鹳** (bái guàn) | White Stork | *Ciconia ciconia*  
- **钳嘴鹳** (qián zuǐ guàn) | Asian Openbill | *Anastomus oscitans*  
- **黑鹳** (hēi guàn) | Black Stork | *Ciconia nigra*

**6. 鹈形目 (Pelecaniformes)**（首字：白 U+767D → 苍 U+82CD）  
- **白鹭** (bái lù) | Little Egret | *Egretta garzetta*  
- **苍鹭** (cāng lù) | Grey Heron | *Ardea cinerea*

**7. 鹰形目 (Accipitriformes)**  
- **大鶚** (dà è) | Osprey | *Pandion haliaetus*

**8. 鸡形目 (Galliformes)**  
- **白鷳** (bái xián) | Silver Pheasant | *Lophura nycthemera*

**9. 佛法僧目 (Coraciiformes)**（首字：白 U+767D → 翠 U+7FE0 → 荷 U+8377）  
- **白胸翡翠鳥** (bái xiōng fěi cuì niǎo) | White-throated Kingfisher | *Halcyon smyrnensis*  
- **翠鸟** (cuì niǎo) | Common Kingfisher | *Alcedo atthis*  
- **荷花翠鸟** (hé huā cuì niǎo) | Black‑capped Kingfisher (蓝翡翠) | *Halcyon pileata*  

**10. 鹦形目 (Psittaciformes)**  
- **彩鹦** (cǎi yīng) | Painted Tiger Parrot | *Psittacella picta*

**11. 雀形目 (Passeriformes)**（首字 Unicode：海 U+6D77 → 火 U+706B → 灰 U+7070 → 红 U+7EA2；同“红”者按第二字：嘴 U+5634 → 尾 U+5C3E → 梅 U+6885）  
- **海南藍仙鶲** (hǎi nán lán xiān wēng) | Hainan Blue Flycatcher | *Cyornis hainanus*  
- **火尾太阳鸟** (huǒ wěi tài yáng niǎo) | Fire-tailed Sunbird | *Aethopyga ignicauda*  
- **灰鹡鸰** (huī jí líng) | Grey Wagtail | *Motacilla cinerea*  
- **紅嘴藍雀** (hóng zuǐ lán què) | Red-billed Blue Magpie | *Urocissa erythroryncha*  
- **紅尾水鴝** (hóng wěi shuǐ qú) | Plumbeous Water Redstart | *Phoenicurus fuliginosus*  
- **紅梅花雀** (hóng méi huā què) | Red Avadavat | *Amandava amandava*

---

### English Section

**Abstract**  
The task is complete. All 27 birds have been grouped into **11 taxonomic orders**, with each order internally sorted by the **Unicode code points of the Chinese names** (from lowest to highest). The original form (traditional or simplified) of each name is preserved exactly as given. The list includes pinyin, English names, and scientific names. For the non‑standard name “荷花翠鸟”, I identified it as **Black‑capped Kingfisher (*Halcyon pileata*)** based on morphology and taxonomic affinity, and it has been placed accordingly.

**Detailed Explanation**

**Sorting logic**: Within each order, compare the full Chinese name string by Unicode code points, starting from the first character. For example, “白” (U+767D) comes before “苍” (U+82CD). The sorted results are as follows (ascending order within each group):

---

**1. Anseriformes**  
- **中华秋沙鸭** (zhōng huá qiū shā yā) | Scaly-sided Merganser | *Mergus squamatus*  
- **青头潜鸭** (qīng tóu qián yā) | Baer's Pochard | *Aythya baeri*

**2. Podicipediformes**  
- **鸊鷉** (pì tī) | Little Grebe | *Tachybaptus ruficollis*

**3. Gruiformes**  
- **白鹤** (bái hè) | Siberian Crane | *Leucogeranus leucogeranus*  
- **黑水鷄** (hēi shuǐ jī) | Common Moorhen | *Gallinula chloropus*

**4. Charadriiformes** (sorted by first character: 大 U+5927 → 燕 U+71D5 → 白 U+767D → 粉 U+7C89 → 黑 U+9ED1)  
- **大鳳頭燕鷗** (dà fèng tóu yàn ōu) | Great Crested Tern | *Thalasseus bergii*  
- **燕鷗** (yàn ōu) | Common Tern | *Sterna hirundo*  
- **白額燕鷗** (bái é yàn ōu) | Little Tern | *Sternula albifrons*  
- **粉紅燕鷗** (fěn hóng yàn ōu) | Roseate Tern | *Sterna dougallii*  
- **黑枕燕鷗** (hēi zhěn yàn ōu) | Black-naped Tern | *Sterna sumatrana*

**5. Ciconiiformes** (first char: 白 U+767D → 钳 U+94B3 → 黑 U+9ED1)  
- **白鹳** (bái guàn) | White Stork | *Ciconia ciconia*  
- **钳嘴鹳** (qián zuǐ guàn) | Asian Openbill | *Anastomus oscitans*  
- **黑鹳** (hēi guàn) | Black Stork | *Ciconia nigra*

**6. Pelecaniformes** (first char: 白 U+767D → 苍 U+82CD)  
- **白鹭** (bái lù) | Little Egret | *Egretta garzetta*  
- **苍鹭** (cāng lù) | Grey Heron | *Ardea cinerea*

**7. Accipitriformes**  
- **大鶚** (dà è) | Osprey | *Pandion haliaetus*

**8. Galliformes**  
- **白鷳** (bái xián) | Silver Pheasant | *Lophura nycthemera*

**9. Coraciiformes** (first char: 白 U+767D → 翠 U+7FE0 → 荷 U+8377)  
- **白胸翡翠鳥** (bái xiōng fěi cuì niǎo) | White-throated Kingfisher | *Halcyon smyrnensis*  
- **翠鸟** (cuì niǎo) | Common Kingfisher | *Alcedo atthis*  
- **荷花翠鸟** (hé huā cuì niǎo) | Black‑capped Kingfisher (Blue‑capped Kingfisher) | *Halcyon pileata*

**10. Psittaciformes**  
- **彩鹦** (cǎi yīng) | Painted Tiger Parrot | *Psittacella picta*

**11. Passeriformes** (first‑char Unicode: 海 U+6D77 → 火 U+706B → 灰 U+7070 → 红 U+7EA2; for those starting with 红, compare second char: 嘴 U+5634 → 尾 U+5C3E → 梅 U+6885)  
- **海南藍仙鶲** (hǎi nán lán xiān wēng) | Hainan Blue Flycatcher | *Cyornis hainanus*  
- **火尾太阳鸟** (huǒ wěi tài yáng niǎo) | Fire-tailed Sunbird | *Aethopyga ignicauda*  
- **灰鹡鸰** (huī jí líng) | Grey Wagtail | *Motacilla cinerea*  
- **紅嘴藍雀** (hóng zuǐ lán què) | Red-billed Blue Magpie | *Urocissa erythroryncha*  
- **紅尾水鴝** (hóng wěi shuǐ qú) | Plumbeous Water Redstart | *Phoenicurus fuliginosus*  
- **紅梅花雀** (hóng méi huā què) | Red Avadavat | *Amandava amandava*

---

## 3. References

[1] 百度百科 – 彩鹦鹉. URL: https://baike.baidu.com/item/彩鹦鹉/10755262  
[2] 百度百科 – 蓝翡翠. URL: https://baike.baidu.com/item/蓝翡翠/10886470  
[3] 百度百科 – 鹗. URL: https://baike.baidu.com/item/鹗/10755263  
[4] 维基百科 – 白胸翡翠. URL: https://zh.wikipedia.org/zh-cn/白胸翡翠