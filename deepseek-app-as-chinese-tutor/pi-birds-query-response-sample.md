\## 1. Prompt Repetition & Chinese Essence



**Original Prompt (English):**  

\> You are a Chinese ornithologist. Here’s a list of birds 「苍鹭,火尾太阳鸟,白鷳,灰鹡鸰,黑水鷄,翠鸟,白鹳,白額燕鷗,黑枕燕鷗,粉紅燕鷗,大鳳頭燕鷗,燕鷗,鸊鷉,钳嘴鹳,白鹭,藍仙鶲,紅梅花雀,紅嘴藍雀,黑鹳,中华秋沙鸭,青头潜鸭,彩鹦,白鹤,紅尾水鴝」。 Add pinyin to each name, the common name in English and scientific name. With this generated list, group the birds by taxonomy, sorting each taxonomy’s birds by the unicode of bird’s Chinese name.. Then return the sorted list to me.



**Chinese Essence (简明口语化):**  

\> 你扮演中国鸟类学家，给你一份24种鸟类的清单（含繁简体）。你要做三件事：加拼音、英文名和拉丁学名；按传统分类（目级）分组；每组内按中文名的Unicode码位从小到大排序（而非拼音）。最后把整理好的清单返回给我。



\---



\## 2. Bilingual Response



\### 中文部分 (Chinese Section)



**摘要**  

已按你的要求整理完毕。共24种鸟类，按**9个目**分组，每组内部严格按照中文名的**Unicode码位（从低到高）** 排序。清单包含拼音、英文名和学名。同时特别说明：“彩鹦”在中国鸟类名录中并无记录，根据字形和鸟类类群推测，极大概率是 **“彩鹮”** (*Plegadis falcinellus*) 的笔误，本次按“彩鹮”处理并纳入鹈形目。



**详细说明**  

排序逻辑：每组内比较中文名的完整字符串（按Unicode码位），从第一个字符开始依次比较。例如“大”(U+5927)排在“燕”(U+71D5)之前。整理结果如下：



**1. 雁形目 (Anseriformes)**

\- **中华秋沙鸭** (zhōng huá qiū shā yā) | Scaly-sided Merganser | *Mergus squamatus*

\- **青头潜鸭** (qīng tóu qián yā) | Baer's Pochard | *Aythya baeri*



**2. 䴙䴘目 (Podicipediformes)**

\- **鸊鷉** (pì tī) | Little Grebe | *Tachybaptus ruficollis*



**3. 鹤形目 (Gruiformes)**

\- **白鹤** (bái hè) | Siberian Crane | *Leucogeranus leucogeranus*

\- **黑水鸡** (hēi shuǐ jī) | Common Moorhen | *Gallinula chloropus*



**4. 鸻形目 (Charadriiformes)**

\- **大凤头燕鸥** (dà fèng tóu yàn ōu) | Great Crested Tern | *Thalasseus bergii*

\- **燕鸥** (yàn ōu) | Common Tern | *Sterna hirundo*

\- **白额燕鸥** (bái é yàn ōu) | Little Tern | *Sternula albifrons*

\- **粉红燕鸥** (fěn hóng yàn ōu) | Roseate Tern | *Sterna dougallii*

\- **黑枕燕鸥** (hēi zhěn yàn ōu) | Black-naped Tern | *Sterna sumatrana*



**5. 鹳形目 (Ciconiiformes)**

\- **白鹳** (bái guàn) | White Stork | *Ciconia ciconia*

\- **钳嘴鹳** (qián zuǐ guàn) | Asian Openbill | *Anastomus oscitans*

\- **黑鹳** (hēi guàn) | Black Stork | *Ciconia nigra*



**6. 鹈形目 (Pelecaniformes)**

\- **彩鹮** (cǎi huán) *(原清单“彩鹦”的订正)* | Glossy Ibis | *Plegadis falcinellus*

\- **白鹭** (bái lù) | Little Egret | *Egretta garzetta*

\- **苍鹭** (cāng lù) | Grey Heron | *Ardea cinerea*



**7. 鸡形目 (Galliformes)**

\- **白鹇** (bái xián) | Silver Pheasant | *Lophura nycthemera*



**8. 佛法僧目 (Coraciiformes)**

\- **翠鸟** (cuì niǎo) | Common Kingfisher | *Alcedo atthis*



**9. 雀形目 (Passeriformes)**（按首字Unicode：火 U+706B → 灰 U+7070 → 红 U+7EA2 → 蓝 U+84DD，同“红”再按第二字U+5634嘴 < U+5C3E尾 < U+6885梅）

\- **火尾太阳鸟** (huǒ wěi tài yáng niǎo) | Fire-tailed Sunbird | *Aethopyga ignicauda*

\- **灰鹡鸰** (huī jí líng) | Grey Wagtail | *Motacilla cinerea*

\- **红嘴蓝鹊** (hóng zuǐ lán què) | Red-billed Blue Magpie | *Urocissa erythroryncha*

\- **红尾水鸲** (hóng wěi shuǐ qú) | Plumbeous Water Redstart | *Phoenicurus fuliginosus*

\- **红梅花雀** (hóng méi huā què) | Red Avadavat | *Amandava amandava*

\- **蓝仙鹟** (lán xiān wēng) | Hainan Blue Flycatcher | *Cyornis hainanus*



\---



\### English Section



**Abstract**  

The task is complete. All 24 birds have been grouped into **9 taxonomic orders**, with each order internally sorted strictly by the **Unicode code points of the Chinese names** (from lowest to highest). The list includes pinyin, English names, and scientific names. Additionally, I have flagged “彩鹦” as a probable typo for **“彩鹮” (Glossy Ibis)**, which has been placed in Pelecaniformes.



**Detailed Explanation**  

Sorting logic: within each group, compare the full Chinese name string by Unicode code points, starting from the first character. For example, “大” (U+5927) comes before “燕” (U+71D5). The sorted results are as follows:



**1. Anseriformes**

\- **中华秋沙鸭** (zhōng huá qiū shā yā) | Scaly-sided Merganser | *Mergus squamatus*

\- **青头潜鸭** (qīng tóu qián yā) | Baer's Pochard | *Aythya baeri*



**2. Podicipediformes**

\- **鸊鷉** (pì tī) | Little Grebe | *Tachybaptus ruficollis*



**3. Gruiformes**

\- **白鹤** (bái hè) | Siberian Crane | *Leucogeranus leucogeranus*

\- **黑水鸡** (hēi shuǐ jī) | Common Moorhen | *Gallinula chloropus*



**4. Charadriiformes**

\- **大凤头燕鸥** (dà fèng tóu yàn ōu) | Great Crested Tern | *Thalasseus bergii*

\- **燕鸥** (yàn ōu) | Common Tern | *Sterna hirundo*

\- **白额燕鸥** (bái é yàn ōu) | Little Tern | *Sternula albifrons*

\- **粉红燕鸥** (fěn hóng yàn ōu) | Roseate Tern | *Sterna dougallii*

\- **黑枕燕鸥** (hēi zhěn yàn ōu) | Black-naped Tern | *Sterna sumatrana*



**5. Ciconiiformes**

\- **白鹳** (bái guàn) | White Stork | *Ciconia ciconia*

\- **钳嘴鹳** (qián zuǐ guàn) | Asian Openbill | *Anastomus oscitans*

\- **黑鹳** (hēi guàn) | Black Stork | *Ciconia nigra*



**6. Pelecaniformes**

\- **彩鹮** (cǎi huán) *(correction of the original “彩鹦”)* | Glossy Ibis | *Plegadis falcinellus*

\- **白鹭** (bái lù) | Little Egret | *Egretta garzetta*

\- **苍鹭** (cāng lù) | Grey Heron | *Ardea cinerea*



**7. Galliformes**

\- **白鹇** (bái xián) | Silver Pheasant | *Lophura nycthemera*



**8. Coraciiformes**

\- **翠鸟** (cuì niǎo) | Common Kingfisher | *Alcedo atthis*



**9. Passeriformes** (sorted by first-character Unicode: 火 U+706B → 灰 U+7070 → 红 U+7EA2 → 蓝 U+84DD; for those starting with 红, sort by second character: U+5634嘴 < U+5C3E尾 < U+6885梅)

\- **火尾太阳鸟** (huǒ wěi tài yáng niǎo) | Fire-tailed Sunbird | *Aethopyga ignicauda*

\- **灰鹡鸰** (huī jí líng) | Grey Wagtail | *Motacilla cinerea*

\- **红嘴蓝鹊** (hóng zuǐ lán què) | Red-billed Blue Magpie | *Urocissa erythroryncha*

\- **红尾水鸲** (hóng wěi shuǐ qú) | Plumbeous Water Redstart | *Phoenicurus fuliginosus*

\- **红梅花雀** (hóng méi huā què) | Red Avadavat | *Amandava amandava*

\- **蓝仙鹟** (lán xiān wēng) | Hainan Blue Flycatcher | *Cyornis hainanus*



\---



\## 3. References



*No external citations were used in this response. The scientific and common names are based on standard Chinese ornithological references (e.g., *A Checklist of the Birds of China*).*