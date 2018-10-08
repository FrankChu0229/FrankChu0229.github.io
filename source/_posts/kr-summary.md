title: Knowledge Representation Summary
date: 2018-10-08 13:22:34
tags: [kg, kr, summary]
categories: Knowledge Graph
description: Knowledge Representation Summary.
---

## 知识表示


### RDF

三元组, 表达能力有限。

### RDFS

在RDF的基础上，添加了

```
Class, subClassOf, type, Property, subPropertyOf, Domain, Range
```
Examples:

```
:hasCharacteristic a rdfs:Property ;
                   rdfs:domain :Person .

:hasHeightInInches rdfs:subPropertyOf :hasCharacteristics ;
                   rdfs:domain xsd:int .

:hasName           rdfs:subPropertyOf :hasCharacteristic ;
                   rdfs:domain xsd:string .
```
### OWL 词汇

```
exp:运动员 owl:equivalentClass exp:体育选手 
exp:获得 owl:equivalentProperty exp:取得
exp:运动员A owl:sameIndividualAs exp:小明
exp:ancestor rdf:type owl:TransitiveProperty
exp:ancestor owl:inverseOf exp:descendant
exp:hasMother rdf:type owl:FunctionalProperty // 每个人只有一个母亲
exp:friend rdf:type owl:SymmetricProperty
...
```
### JsonLD

JsonLD 是一 种基于JSON表示和传输互联数据 (Linked Data)的方法, 在java、python等语言中，都有对jsonld的解析lib。

```
{
"http://schema.org/name": "Manu Sporny",
"http://schema.org/url": { "@id":"http://manu.sporny.org/" }, "http://schema.org/image": { "@id":"http://manu.sporny.org/images/manu.png" }
}
```
还有RDFa等，算是对知识的一种新型表示方法。

### 各种典型知识图谱项目中的知识表示

#### WordNet
- linguistic 图谱
- 词之间语言学关系 (同义词、反义词、上位词、下位词)

中文词汇网路 (Chinese Wordnet, 以下简称中文词网) 计画，目的是在提供完整的中文词义 (sense) 区分与词汇语意关系知识库。相信词义的区分与表达，必须建立在完善的词汇语意学 (lexical semantics) 理论与知识本体 (ontology) 架构基础上。在词义理论与认知研究方面，这个详细分析的词汇知识库系统，将成為语言学研究的基本参考资料。在实际的应用上，这个资料库可望成為中文语言处理与知识工程不可或缺的基底架构。

本计划自 2003 年起，迄今累积了近十年的研究成果，对词义区分定义，与词义知识表达方式，渐次做了修正。建构过程中，也曾发表於国内外相关研究机关与数个国际研讨会议，得到了许多有价值的建议。中文词网的网路搜寻介面，在 2006 年於中央研究院语言学研究所正式啟用，提供给各界检索使用。到 2010 计画执行结束前，网站资料与技术报告内容皆作同步更新。为了永续经营此项珍贵的中文词汇资源，目前计画网站转由国立台湾大学语言学研究所维护。

#### DbPedia && CN-Depedia

百科知识图谱
CN-Depedia 整合了如百度百科、互动百科、中文维基百科等百科网站

#### YAGO

 多语言知识库，包含中文。

YAGO是由德国马普研究所研制的链接数据库。YAGO主要集成了Wikipedia、WordNet和GeoNames三个来源的数据。YAGO将WordNet的词汇定义与Wikipedia的分类体系进行了融合集成，使得YAGO具有更加丰富的实体分类体系。YAGO还考虑了时间和空间知识，为很多知识条目增加了时间和空间维度的属性描述。目前，YAGO包含1.2亿条三元组知识。YAGO是IBM Watson的后端知识库之一。

#### Probase && CN-Probase

- 通用概念知识图谱	
- 包含约1700万实体、27万概念和3300万isa关系。
- schema: Class, Entity, Property:isA (only), 

#### FreeBase

类似于wikidata，由社区等众包而成，


Freebase is built on the notions of objects, facts, types, and properties. Each Freebase object has a stable identifier called a “mid” (for Machine ID), one or more types, and uses properties from these types in order to provide facts. For example, the Freebase object for Barack Obama has the mid /m/02mjmr and the type /government/us_president (among others) that allows the entity to have a fact with the property /government/us_president/presidency_number and the literal integer “44” as the value. 

Freebase uses Compound Value Types (CVTs) to represent n-ary relations with n > 2, e.g., values like geographic coordinates, political positions held with a start and an end date (see Figure 1 for an example), or actors playing a character in a movie. CVT values are just objects, i.e., they have a mid and can have types (although they usually only have the compound value type itself). Most non-CVT objects are called topics in order to discern them from CVTs.

The content of Freebase has been partially imported from various sources such as Wikipedia [1] or the license-compatible part of MusicBrainz [30]. Over the years, the Freebase community and Google have maintained the knowledge base. When Freebase was turned read-only on March 31, 2015, it counted more than 3 billion facts about almost 50 million entities. Freebase data is published as an N-Triples dump in RDF [6] under the Creative Commons CC-BY license.


#### zhishi.me 

- 百科知识图谱(中文百科，百度百科，互动百科)
- 通用领域知识图谱

[Zhishi.me](http://zhishi.me/) 通过从开放的百科数据中抽取结构化数据，首次尝试构建中文通用知识图谱。目前，已融合了三大中文百科，百度百科，互动百科以及维基百科中的数据。
#### WikiData

WikiData的目标是构建一个免费开放、多语言、任何人或机器都可以编辑修改的大规模链接知识库。WikiData由维基百科于2012年启动，早期得到微软联合创始人Paul Allen、Gordon Betty Moore基金会以及Google的联合资助。WikiData继承了Wikipedia的众包协作的机制，但与Wikipedia不同，WikiData支持的是以三元组为基础的知识条目（Items）的自由编辑。一个三元组代表一个关于该条目的陈述（Statements）。例如可以给“地球”的条目增加“”的三元组陈述。截止2016年，WikiData已经包含超过2470多万个知识条目。

Wikidata’s data model relies on the notions of item and statement. An item represents an entity, has a stable identifier called “qid”, and may have labels, descriptions, and aliases in multiple languages; further statements and links to pages about the entity in other Wikimedia projects—most prominently Wikipedia. Contrary to Freebase, Wikidata statements do not aim to encode true facts, but claims from different sources, which can also contradict each other, which, for example, allows for border conflicts to be expressed from different political points of view.

**中文dump [zh-onto](http://openkg.cn/dataset/zhonto)**

#### BabelNet

**多语言词典知识库**

BabelNet是类似于WordNet的多语言词典知识库。BabelNet的目标是解决WordNet在非英语语种中数据缺乏的问题。BabelNet采用的方法是将WordNet词典与Wikipedia百科集成。首先建立WordNet中的词与Wikipedia的页面标题的映射，然后利用Wikipedia中的多语言链接，再辅以机器翻译技术，来给WordNet增加多种语言的词汇。BabelNet3.7包含了271种语言，1400万同义词组，36.4万词语关系和3.8亿从Wikipedia中抽取的链接关系，总计超过19亿RDF三元组。 BabelNet集成了WordNet在词语关系上的优势和Wikipedia在多语言语料方面的优势，构建成功了目前最大规模的多语言词典知识库。

#### ConceptNet

**多语言常识知识库**

ConceptNet是常识知识库。最早源于MIT媒体实验室的Open Mind Common Sense (OMCS)项目。OMCS项目是由著名人工智能专家Marvin Minsky于1999年建议创立。ConceptNet主要依靠互联网众包、专家创建和游戏三种方法来构建。ConceptNet知识库以三元组形式的关系型知识构成。ConceptNet5版本已经包含有2800万关系描述。与Cyc相比，ConceptNet采用了非形式化、更加接近自然语言的描述，而不是像Cyc那样采用形式化的谓词逻辑。与链接数据和谷歌知识图谱相比，ConceptNet比较侧重于词与词之间的关系。从这个角度看，ConceptNet更加接近于WordNet，但是又比WordNet包含的关系类型多。此外，ConceptNet完全免费开放，并支持多种语言。

Cyc的扩展

#### NELL

openIE的一种

#### Schema.org && CN-Schema

[cnSchema.org](http://cnschema.org/)是一个基于社区维护的开放的知识图谱Schema标准。cnSchema的词汇集包括了上千种概念分类(classes)、数据类型(data types)、属性(propertities)和关系(relations)等常用概念定义，以支持知识图谱数据的通用性、复用性和流动性。结合中文的特点，我们复用、连接并扩展了[Schema.org](http://schema.org/)，Wikidata， Wikipedia等已有的知识图谱Schema标准，为中文领域的开放知识图谱、聊天机器人、搜索引擎优化等提供可供参考和扩展的数据描述和接口定义标准。通过cnSchema, 开发者也可以快速对接上百万基于[Schema.org](http://schema.org/)定义的网站，以及Bot的知识图谱数据API。


## Reference

- [OpenKG](http://openkg.cn/home)
- [From Freebase to Wikidata: The Great Migration](https://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/44818.pdf)

