---
title: Book Recommendations engine from Charles Darwin
date: 2022-09-28T13:04:04.925Z
summary: Are you a Charles Darwin fan and finding another similar book from him?
  Let's find one then.
draft: false
featured: false
tags:
  - NLP
  - UNSUPERVISED
categories:
  - EDA
image:
  filename: https://i0.wp.com/nabanitadhar.in/wp-content/uploads/2018/07/Something-in-the-water.jpg?fit=1000%2C667&ssl=1
  focal_point: Smart
  preview_only: true
---
## 1. Darwin's bibliography
<p><img src="https://assets.datacamp.com/production/project_607/img/CharlesDarwin.jpg" alt="Charles Darwin" width="300px"></p>
<p>Charles Darwin is one of the few universal figures of science. His most renowned work is without a doubt his "<em>On the Origin of Species</em>" published in 1859 which introduced the concept of natural selection. But Darwin wrote many other books on a wide range of topics, including geology, plants or his personal life. In this notebook, we will automatically detect how closely related his books are to each other.</p>
<p>To this purpose, we will develop the bases of <strong>a content-based book recommendation system</strong>, which will determine which books are close to each other based on how similar the discussed topics are. The methods we will use are commonly used in text- or documents-heavy industries such as legal, tech or customer support to perform some common task such as text classification or handling search engine queries.</p>
<p>Let's take a look at the books we'll use in our recommendation system.</p>


```python
# Import library
import glob

# The books files are contained in this folder
folder = "datasets/"

# List all the .txt files and sort them alphabetically
files = glob.glob(folder+'*.txt',recursive=True)

files
```
`﻿``
    ['datasets/Autobiography.txt',
     'datasets/CoralReefs.txt',
     'datasets/DescentofMan.txt',
     'datasets/DifferentFormsofFlowers.txt',
     'datasets/EffectsCrossSelfFertilization.txt',
     'datasets/ExpressionofEmotionManAnimals.txt',
     'datasets/FormationVegetableMould.txt',
     'datasets/FoundationsOriginofSpecies.txt',
     'datasets/GeologicalObservationsSouthAmerica.txt',
     'datasets/InsectivorousPlants.txt',
     'datasets/LifeandLettersVol1.txt',
     'datasets/LifeandLettersVol2.txt',
     'datasets/MonographCirripedia.txt',
     'datasets/MonographCirripediaVol2.txt',
     'datasets/MovementClimbingPlants.txt',
     'datasets/OriginofSpecies.txt',
     'datasets/PowerMovementPlants.txt',
     'datasets/VariationPlantsAnimalsDomestication.txt',
     'datasets/VolcanicIslands.txt',
     'datasets/VoyageBeagle.txt']
`﻿``


## 2. Load the contents of each book into Python
<p>As a first step, we need to load the content of these books into Python and do some basic pre-processing to facilitate the downstream analyses. We call such a collection of texts <strong>a corpus</strong>. We will also store the titles for these books for future reference and print their respective length to get a gauge for their contents.</p>


```python
# Import libraries
import re, os

# Initialize the object that will contain the texts and titles
txts = []
titles = []
T = True

for n in files:
    # Open each file
    f = open(n, encoding='utf-8-sig')
    # Remove all non-alpha-numeric characters
    filtered = re.sub('[\W_]+', ' ', f.read())
    # Store the texts and titles of the books in two separate lists
    txts.append(filtered)
    titles.append(os.path.basename(n).replace(".txt", ""))

# Print the length, in characters, of each book
[len(t) for t in txts]
```

`﻿``
    [123231,
     496068,
     1776539,
     617088,
     913713,
     624232,
     335920,
     523021,
     797401,
     901406,
     1047518,
     1010643,
     767492,
     1660866,
     298319,
     916267,
     1093567,
     1043499,
     341447,
     1149574]
`﻿``


## 3. Find "On the Origin of Species"
<p>For the next parts of this analysis, we will often check the results returned by our method for a given book. For consistency, we will refer to Darwin's most famous book: "<em>On the Origin of Species</em>." Let's find to which index this book is associated.</p>


```python
# Browse the list containing all the titles
# print(titles[:2])
ori = 0
for i in range(len(titles)):
    # Store the index if the title is "OriginofSpecies"
    if titles[i] == 'OriginofSpecies':
        ori = i 

# Print the stored index
print(ori)
```
```
    15
`﻿``

## 4. Tokenize the corpus
<p>As a next step, we need to transform the corpus into a format that is easier to deal with for the downstream analyses. We will tokenize our corpus, i.e., transform each text into a list of the individual words (called tokens) it is made of. To check the output of our process, we will print the first 20 tokens of "<em>On the Origin of Species</em>".</p>


```python
# Define a list of stop words
stoplist = set('for a of the and to in to be which some is at that we i who whom show via may my our might as well'.split())

# Convert the text to lower case 
txts_lower_case = [txt.lower() for txt in txts]


# Transform the text into tokens 
txts_split = [txt.split() for txt in txts_lower_case]

# Remove tokens which are part of the list of stop words
texts = []
for txts in txts_split:
    temp = []
    for token in txts:
        if token not in stoplist:
            temp.append(token)
    texts.append(temp)
    
# Print the first 20 tokens for the "On the Origin of Species" book
print(texts[ori][:20])
```

`﻿``
    ['on', 'origin', 'species', 'but', 'with', 'regard', 'material', 'world', 'can', 'least', 'go', 'so', 'far', 'this', 'can', 'perceive', 'events', 'are', 'brought', 'about']
`﻿``

## 5. Stemming of the tokenized corpus
<p>If you have read <em>On the Origin of Species</em>, you will have noticed that Charles Darwin can use different words to refer to a similar concept. For example, the concept of selection can be described by words such as <em>selection</em>, <em>selective</em>, <em>select</em> or <em>selects</em>. This will dilute the weight given to this concept in the book and potentially bias the results of the analysis.</p>
<p>To solve this issue, it is a common practice to use a <strong>stemming process</strong>, which will group together the inflected forms of a word so they can be analysed as a single item: <strong>the stem</strong>. In our <em>On the Origin of Species</em> example, the words related to the concept of selection would be gathered under the <em>select</em> stem.</p>
<p>As we are analysing 20 full books, the stemming algorithm can take several minutes to run and, in order to make the process faster, we will directly load the final results from a pickle file and review the method used to generate it.</p>


```python
import pickle

# Load the Porter stemming function from the nltk package
# from nltk.stem import PorterStemmer

# Create an instance of a PorterStemmer object
# porter = PorterStemmer()

# For each token of each text, we generated its stem 
# texts_stem = [[porter.stem(token) for token in text] for text in texts]

# Save to pickle file
# pickle.dump(texts_stem, open("datasets/texts_stem.p", "wb" ))

# Load the stemmed tokens list from the pregenerated pickle file
texts_stem = pickle.load(open('datasets/texts_stem.p', 'rb'))

# Print the 20 first stemmed tokens from the "On the Origin of Species" book
print(texts_stem[ori][:20])
```
`﻿``
    ['on', 'origin', 'speci', 'but', 'with', 'regard', 'materi', 'world', 'can', 'least', 'go', 'so', 'far', 'thi', 'can', 'perceiv', 'event', 'are', 'brought', 'about']
`﻿``

## 6. Building a bag-of-words model
<p>Now that we have transformed the texts into stemmed tokens, we need to build models that will be useable by downstream algorithms.</p>
<p>First, we need to will create a universe of all words contained in our corpus of Charles Darwin's books, which we call <em>a dictionary</em>. Then, using the stemmed tokens and the dictionary, we will create <strong>bag-of-words models</strong> (BoW) of each of our texts. The BoW models will represent our books as a list of all uniques tokens they contain associated with their respective number of occurrences. </p>
<p>To better understand the structure of such a model, we will print the five first elements of one of the "<em>On the Origin of Species</em>" BoW model.</p>


```python
# Load the functions allowing to create and use dictionaries
from gensim import corpora

# Create a dictionary from the stemmed tokens
dictionary = corpora.Dictionary()

# Create a bag-of-words model for each book, using the previously generated dictionary
bows = [dictionary.doc2bow(doc,allow_update=True) for doc in texts_stem]

# Print the first five elements of the On the Origin of species' BoW model
bows[ori][:5]
```

`﻿``
    [(0, 11), (5, 51), (6, 1), (8, 2), (21, 1)]
`﻿``


## 7. The most common words of a given book
<p>The results returned by the bag-of-words model is certainly easy to use for a computer but hard to interpret for a human. It is not straightforward to understand which stemmed tokens are present in a given book from Charles Darwin, and how many occurrences we can find.</p>
<p>In order to better understand how the model has been generated and visualize its content, we will transform it into a DataFrame and display the 10 most common stems for the book "<em>On the Origin of Species</em>".</p>


```python
# Import pandas to create and manipulate DataFrames
import pandas as pd

# Convert the BoW model for "On the Origin of Species" into a DataFrame
df_bow_origin = pd.DataFrame(bows[ori])

# Add the column names to the DataFrame
df_bow_origin.columns = ['index','occurrences']

# Add a column containing the token corresponding to the dictionary index
df_bow_origin['token'] = [dictionary[idx] for idx in df_bow_origin['index']]

# Sort the DataFrame by descending number of occurrences and print the first 10 values
df_bow_origin.sort_values('occurrences',ascending = False).head(10)
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>occurrences</th>
      <th>token</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>748</th>
      <td>1168</td>
      <td>2023</td>
      <td>have</td>
    </tr>
    <tr>
      <th>1119</th>
      <td>1736</td>
      <td>1558</td>
      <td>on</td>
    </tr>
    <tr>
      <th>1489</th>
      <td>2288</td>
      <td>1543</td>
      <td>speci</td>
    </tr>
    <tr>
      <th>892</th>
      <td>1366</td>
      <td>1480</td>
      <td>it</td>
    </tr>
    <tr>
      <th>239</th>
      <td>393</td>
      <td>1362</td>
      <td>by</td>
    </tr>
    <tr>
      <th>1128</th>
      <td>1747</td>
      <td>1201</td>
      <td>or</td>
    </tr>
    <tr>
      <th>125</th>
      <td>218</td>
      <td>1140</td>
      <td>are</td>
    </tr>
    <tr>
      <th>665</th>
      <td>1043</td>
      <td>1137</td>
      <td>from</td>
    </tr>
    <tr>
      <th>1774</th>
      <td>2703</td>
      <td>1000</td>
      <td>with</td>
    </tr>
    <tr>
      <th>1609</th>
      <td>2452</td>
      <td>962</td>
      <td>thi</td>
    </tr>
  </tbody>
</table>
</div>



## 8. Build a tf-idf model
<p>If it wasn't for the presence of the stem "<em>speci</em>", we would have a hard time to guess this BoW model comes from the <em>On the Origin of Species</em> book. The most recurring words are, apart from few exceptions, very common and unlikely to carry any information peculiar to the given book. We need to use an additional step in order to determine which tokens are the most specific to a book.</p>
<p>To do so, we will use a <strong>tf-idf model</strong> (term frequency–inverse document frequency). This model defines the importance of each word depending on how frequent it is in this text and how infrequent it is in all the other documents. As a result, a high tf-idf score for a word will indicate that this word is specific to this text.</p>
<p>After computing those scores, we will print the 10 words most specific to the "<em>On the Origin of Species</em>" book (i.e., the 10 words with the highest tf-idf score).</p>


```python
# Load the gensim functions that will allow us to generate tf-idf models
from gensim.models import TfidfModel

# Generate the tf-idf model
model = TfidfModel(bows)

# Print the model for "On the Origin of Species"
model[bows[ori]]
```

`﻿``
    [(8, 0.00020383224047642202),
     (21, 0.0005716037746542094),
     (23, 0.0017118699041370883),
     (27, 0.0006458270601429994),
     (28, 0.0025678048562056324),
     (31, 0.0008559349520685442),
     (35, 0.00101497410751472),
     (36, 0.00101497410751472),
     (51, 0.000886740665721021),
     (54, 0.00202994821502944),
     (56, 0.0023757190244598344),
     (57, 0.00010191612023821101),
     (63, 0.0027544680933525786),
     (64, 0.000509580601191055),
     (66, 0.00020383224047642202),
     (67, 0.0023757190244598344),
     (68, 0.00202994821502944),
     (75, 0.0013772340466762893),
     (76, 0.0004433703328605105),
     (78, 0.004171843479607349),
     (80, 0.0020859217398036746),
     (83, 0.00857405661981314),
     (84, 0.000509580601191055),
     (88, 0.002445986885717064),
     (89, 0.0033632319678609636),
     (90, 0.000886740665721021),
     (91, 0.0016747506839411234),
     (94, 0.000886740665721021),
     (95, 0.0004433703328605105),
     (96, 0.003546962662884084),
     (97, 0.0016306579238113761),
     (102, 0.037686478293143394),
     (104, 0.000917245082143899),
     (106, 0.001417375386254771),
     (108, 0.0035434384656369273),
     (109, 0.005299638252386972),
     (111, 0.0022421546452406423),
     (114, 0.0015287418035731652),
     (123, 0.0509769226270304),
     (125, 0.009580115302391834),
     (126, 0.004171843479607349),
     (127, 0.001417375386254771),
     (137, 0.020026648174904797),
     (139, 0.007749924721715993),
     (141, 0.00101916120238211),
     (143, 0.004751438048919669),
     (144, 0.0047597336465067894),
     (154, 0.0010467191774632021),
     (156, 0.013962508472634907),
     (165, 0.006781184131501494),
     (167, 0.000611496721429266),
     (172, 0.021771758891234606),
     (176, 0.0033632319678609636),
     (178, 0.0031035923300235736),
     (186, 0.009274366941677202),
     (188, 0.0016747506839411234),
     (192, 0.006257765219411025),
     (196, 0.0038749623608579963),
     (197, 0.0013772340466762893),
     (198, 0.000886740665721021),
     (204, 0.0042521261587643135),
     (207, 0.0022603947105004976),
     (212, 0.001324909563096743),
     (214, 0.020395035311583484),
     (215, 0.021720943436859957),
     (219, 0.0019374811804289981),
     (220, 0.004396220545345449),
     (221, 0.0054618131386103995),
     (222, 0.0018206043795367997),
     (223, 0.0007086876931273855),
     (224, 0.007703414568616897),
     (226, 0.0030574836071463303),
     (230, 0.005135609712411265),
     (231, 0.0027544680933525786),
     (235, 0.002445986885717064),
     (236, 0.0012916541202859988),
     (237, 0.0020934383549264042),
     (241, 0.0029308136968969663),
     (242, 0.0015865778821689297),
     (243, 0.008263404280057736),
     (245, 0.004520789421000995),
     (246, 0.003465148088099174),
     (247, 0.001732574044049587),
     (249, 0.0018840945194337638),
     (251, 0.008343686959214698),
     (252, 0.0030449223225441605),
     (253, 0.0006458270601429994),
     (261, 0.001324909563096743),
     (269, 0.0006458270601429994),
     (271, 0.0008373753419705617),
     (276, 0.009921627703783398),
     (278, 0.0034296226479252566),
     (280, 0.0021260630793821567),
     (283, 0.02138147122013851),
     (285, 0.001417375386254771),
     (287, 0.0013301109985815313),
     (288, 0.006886170233381447),
     (290, 0.0035434384656369273),
     (291, 0.0018206043795367997),
     (296, 0.009839160268154101),
     (298, 0.042694255446964965),
     (300, 0.0016747506839411234),
     (301, 0.000611496721429266),
     (302, 0.0042521261587643135),
     (303, 0.004171843479607349),
     (304, 0.0020859217398036746),
     (311, 0.0016306579238113761),
     (313, 0.007086876931273855),
     (323, 0.0047597336465067894),
     (325, 0.021606217490500734),
     (327, 0.001732574044049587),
     (329, 0.01790404260679176),
     (335, 0.0004433703328605105),
     (336, 0.0023922081541910096),
     (338, 0.0020859217398036746),
     (339, 0.001417375386254771),
     (344, 0.001417375386254771),
     (345, 0.011527628654373272),
     (346, 0.00020383224047642202),
     (348, 0.0006280315064779214),
     (349, 0.0006458270601429994),
     (351, 0.0036412087590735995),
     (354, 0.007980665991489189),
     (356, 0.0018840945194337638),
     (358, 0.013772340466762893),
     (359, 0.0022603947105004976),
     (362, 0.00101497410751472),
     (367, 0.0023922081541910096),
     (369, 0.19772097392783367),
     (370, 0.043694505108883196),
     (371, 0.000509580601191055),
     (372, 0.0023922081541910096),
     (373, 0.0016306579238113761),
     (374, 0.001732574044049587),
     (375, 0.001936406284526009),
     (376, 0.006489658900271853),
     (377, 0.0030449223225441605),
     (380, 0.0016145676503574987),
     (387, 0.0007086876931273855),
     (388, 0.0042521261587643135),
     (389, 0.0005716037746542094),
     (391, 0.0023922081541910096),
     (400, 0.00202994821502944),
     (406, 0.013772340466762893),
     (407, 0.0005716037746542094),
     (409, 0.0035588452033748874),
     (411, 0.0007086876931273855),
     (412, 0.004751438048919669),
     (418, 0.0027544680933525786),
     (421, 0.0027544680933525786),
     (424, 0.007538884401734597),
     (425, 0.00811979286011776),
     (426, 0.0032291353007149973),
     (429, 0.000886740665721021),
     (431, 0.000886740665721021),
     (432, 0.010149741075147201),
     (433, 0.0016306579238113761),
     (434, 0.00405989643005888),
     (436, 0.0021260630793821567),
     (442, 0.009212940010656012),
     (446, 0.004171843479607349),
     (448, 0.021562415055741965),
     (449, 0.03291890683694216),
     (450, 0.0035434384656369273),
     (453, 0.0011210773226203211),
     (454, 0.00405989643005888),
     (456, 0.011301973552502492),
     (457, 0.0020859217398036746),
     (458, 0.0003229135300714997),
     (463, 0.01790404260679176),
     (464, 0.0027544680933525786),
     (465, 0.0008559349520685442),
     (468, 0.00202994821502944),
     (470, 0.0018206043795367997),
     (478, 0.010923626277220799),
     (482, 0.0344479258546676),
     (484, 0.013772340466762893),
     (486, 0.0020859217398036746),
     (489, 0.0054618131386103995),
     (490, 0.022603947105004983),
     (491, 0.00405989643005888),
     (493, 0.0007086876931273855),
     (497, 0.012839024281028162),
     (498, 0.0035434384656369273),
     (502, 0.00637818923814647),
     (505, 0.011970998987233783),
     (507, 0.0009687405902144991),
     (514, 0.0025121260259116855),
     (520, 0.004280477050004863),
     (524, 0.0020859217398036746),
     (526, 0.005716037746542095),
     (527, 0.0027544680933525786),
     (529, 0.006318799454769082),
     (531, 0.003546962662884084),
     (532, 0.012791353704852355),
     (534, 0.005320443994326125),
     (536, 0.0014268256833349542),
     (541, 0.0011432075493084189),
     (543, 0.01110604517518251),
     (544, 0.0031401575323896065),
     (546, 0.0017148113239626283),
     (551, 0.004784416308382019),
     (552, 0.0031731557643378595),
     (558, 0.0023922081541910096),
     (559, 0.00020383224047642202),
     (561, 0.0022421546452406423),
     (562, 0.005197722132148762),
     (563, 0.006908346571257135),
     (564, 0.0011432075493084189),
     (565, 0.00405989643005888),
     (566, 0.0013772340466762893),
     (569, 0.04034670029030646),
     (573, 0.010630315396910783),
     (576, 0.0025121260259116855),
     (578, 0.0028580188732710474),
     (579, 0.003159399727384541),
     (582, 0.0035434384656369273),
     (586, 0.003563578536689751),
     (590, 0.0017118699041370883),
     (592, 0.0010467191774632021),
     (593, 0.0018206043795367997),
     (594, 0.015898914757160917),
     (596, 0.0015865778821689297),
     (598, 0.0023922081541910096),
     (600, 0.01251553043882205),
     (601, 0.00202994821502944),
     (604, 0.05582357591330961),
     (605, 0.00637818923814647),
     (606, 0.0005716037746542094),
     (616, 0.0010467191774632021),
     (619, 0.00710481875260304),
     (620, 0.024945049756828264),
     (626, 0.0013772340466762893),
     (628, 0.00041868767098528084),
     (629, 0.004197875890929496),
     (633, 0.0013301109985815313),
     (635, 0.004960813851891699),
     (636, 0.005991544664479809),
     (641, 0.006346311528675719),
     (646, 0.0027544680933525786),
     (649, 0.005763814327186636),
     (652, 0.000917245082143899),
     (653, 0.0011432075493084189),
     (654, 0.0023757190244598344),
     (655, 0.0016747506839411234),
     (657, 0.0030449223225441605),
     (658, 0.007104097661572994),
     (660, 0.005144433971887885),
     (662, 0.003563578536689751),
     (663, 0.0011878595122299172),
     (665, 0.00101497410751472),
     (666, 0.006395676852426178),
     (667, 0.01623958572023552),
     (668, 0.005669501545019084),
     (670, 0.011723254787587865),
     (674, 0.002445986885717064),
     (675, 0.0450089246309177),
     (676, 0.061655829302082535),
     (678, 0.00040766448095284403),
     (679, 0.005144433971887885),
     (681, 0.0007134128416674771),
     (682, 0.0015865778821689297),
     (685, 0.0020859217398036746),
     (686, 0.0006458270601429994),
     (693, 0.0035670642083373855),
     (698, 0.0022864150986168378),
     (705, 0.0034237398082741766),
     (712, 0.0017118699041370883),
     (713, 0.0013772340466762893),
     (720, 0.0029308136968969663),
     (722, 0.005074870537573601),
     (726, 0.01771719232818464),
     (727, 0.004396220545345449),
     (728, 0.0031731557643378595),
     (729, 0.006089844645088321),
     (730, 0.01028886794377577),
     (731, 0.0017148113239626283),
     (740, 0.008357121859533303),
     (741, 0.0066990027357644935),
     (743, 0.006930296176198348),
     (744, 0.01033323296228799),
     (745, 0.006257765219411025),
     (748, 0.017254559827750243),
     (750, 0.07127157073379503),
     (752, 0.1459896647842414),
     (753, 0.06980942681543291),
     (758, 0.004197875890929496),
     (769, 0.0017118699041370883),
     (770, 0.0010467191774632021),
     (771, 0.0007134128416674771),
     (776, 0.00020934383549264042),
     (783, 0.006346311528675719),
     (784, 0.004171843479607349),
     (785, 0.002834750772509542),
     (786, 0.004171843479607349),
     (787, 0.006930296176198348),
     (788, 0.002955567486908119),
     (789, 0.003546962662884084),
     (793, 0.014208195323145987),
     (797, 0.004197875890929496),
     (798, 0.024359378580353284),
     (803, 0.010149741075147201),
     (805, 0.0034237398082741766),
     (806, 0.02571547930590961),
     (810, 0.0011878595122299172),
     (811, 0.006886170233381447),
     (815, 0.0018840945194337638),
     (816, 0.0012560630129558428),
     (817, 0.0015865778821689297),
     (819, 0.0026602219971630626),
     (820, 0.006458270601429995),
     (821, 0.006207184660047147),
     (822, 0.0688958517093352),
     (825, 0.0003229135300714997),
     (830, 0.001222993442858532),
     (831, 0.003552048830786497),
     (832, 0.02256933073236843),
     (833, 0.01097906002243099),
     (834, 0.0017118699041370883),
     (838, 0.001936406284526009),
     (839, 0.0016306579238113761),
     (842, 0.0013772340466762893),
     (848, 0.006886170233381447),
     (849, 0.0011432075493084189),
     (851, 0.0020859217398036746),
     (857, 0.0005716037746542094),
     (859, 0.008343686959214698),
     (861, 0.0018840945194337638),
     (862, 0.001732574044049587),
     (866, 0.004433703328605105),
     (867, 0.004784416308382019),
     (870, 0.0003229135300714997),
     (873, 0.01886292456358891),
     (875, 0.0009687405902144991),
     (878, 0.00202994821502944),
     (879, 0.005489530011215495),
     (881, 0.0007086876931273855),
     (883, 0.0008153289619056881),
     (885, 0.0023922081541910096),
     (887, 0.001732574044049587),
     (891, 0.0006458270601429994),
     (892, 0.0013772340466762893),
     (894, 0.002445986885717064),
     (897, 0.005991544664479809),
     (898, 0.0011432075493084189),
     (903, 0.0011878595122299172),
     (905, 0.0025678048562056324),
     (909, 0.002649819126193486),
     (910, 0.005508936186705157),
     (917, 0.05877026247301295),
     (919, 0.0017118699041370883),
     (921, 0.0011210773226203211),
     (924, 0.0054015543726251836),
     (925, 0.004784416308382019),
     (929, 0.013718490591701027),
     (931, 0.00101497410751472),
     (935, 0.0026602219971630626),
     (936, 0.0025121260259116855),
     (937, 0.0009687405902144991),
     (939, 0.0016306579238113761),
     (944, 0.000886740665721021),
     (945, 0.00710481875260304),
     (948, 0.0023757190244598344),
     (951, 0.012692623057351438),
     (952, 0.006089844645088321),
     (953, 0.005074870537573601),
     (956, 0.010149741075147201),
     (957, 0.0012560630129558428),
     (958, 0.0008559349520685442),
     (962, 0.0011432075493084189),
     (966, 0.12753430785821307),
     (967, 0.047703783053191846),
     (971, 0.0019374811804289981),
     (973, 0.000509580601191055),
     (975, 0.00040766448095284403),
     (976, 0.00203832240476422),
     (980, 0.009502876097839338),
     (981, 0.016526808560115472),
     (982, 0.00101497410751472),
     (985, 0.03886905667648624),
     (988, 0.004382393170243073),
     (992, 0.0014268256833349542),
     (994, 0.007932889410844648),
     (995, 0.021725146310165012),
     (996, 0.011527628654373272),
     (997, 0.0022603947105004976),
     (998, 0.03014551231094022),
     (999, 0.009519467293013579),
     (1000, 0.00010191612023821101),
     (1004, 0.0011878595122299172),
     ...]
`﻿``


## 9. The results of the tf-idf model
<p>Once again, the format of those results is hard to interpret for a human. Therefore, we will transform it into a more readable version and display the 10 most specific words for the "<em>On the Origin of Species</em>" book.</p>


```python
# Convert the tf-idf model for "On the Origin of Species" into a DataFrame
df_tfidf = pd.DataFrame(model[bows[ori]])

# Name the columns of the DataFrame id and score
df_tfidf.columns = ['id','score']

# Add the tokens corresponding to the numerical indices for better readability
df_tfidf['token'] = [dictionary[idx] for idx in df_tfidf.id]

# Sort the DataFrame by descending tf-idf score and print the first 10 rows.
df_tfidf.sort_values('score',ascending = False).head(10)
```


<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>score</th>
      <th>token</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>878</th>
      <td>2164</td>
      <td>0.327414</td>
      <td>select</td>
    </tr>
    <tr>
      <th>3106</th>
      <td>10108</td>
      <td>0.203908</td>
      <td>pigeon</td>
    </tr>
    <tr>
      <th>128</th>
      <td>369</td>
      <td>0.197721</td>
      <td>breed</td>
    </tr>
    <tr>
      <th>2988</th>
      <td>9396</td>
      <td>0.167496</td>
      <td>migrat</td>
    </tr>
    <tr>
      <th>945</th>
      <td>2325</td>
      <td>0.148186</td>
      <td>steril</td>
    </tr>
    <tr>
      <th>284</th>
      <td>752</td>
      <td>0.145990</td>
      <td>domest</td>
    </tr>
    <tr>
      <th>503</th>
      <td>1255</td>
      <td>0.128272</td>
      <td>hybrid</td>
    </tr>
    <tr>
      <th>370</th>
      <td>966</td>
      <td>0.127534</td>
      <td>fertil</td>
    </tr>
    <tr>
      <th>3889</th>
      <td>16210</td>
      <td>0.124392</td>
      <td>rtner</td>
    </tr>
    <tr>
      <th>3540</th>
      <td>12715</td>
      <td>0.121197</td>
      <td>naturalis</td>
    </tr>
  </tbody>
</table>
</div>



## 10. Compute distance between texts
<p>The results of the tf-idf algorithm now return stemmed tokens which are specific to each book. We can, for example, see that topics such as selection, breeding or domestication are defining "<em>On the Origin of Species</em>" (and yes, in this book, Charles Darwin talks quite a lot about pigeons too). Now that we have a model associating tokens to how specific they are to each book, we can measure how related to books are between each other.</p>
<p>To this purpose, we will use a measure of similarity called <strong>cosine similarity</strong> and we will visualize the results as a distance matrix, i.e., a matrix showing all pairwise distances between Darwin's books.</p>


```python
# Load the library allowing similarity computations
from gensim import similarities

# Compute the similarity matrix (pairwise distance between all texts)
sims = similarities.MatrixSimilarity(model[bows])

# Transform the resulting list into a dataframe
sim_df = pd.DataFrame(list(sims))

# Add the titles of the books as columns and index of the dataframe
sim_df.index = titles
sim_df.columns = titles

# Print the resulting matrix
sim_df
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Autobiography</th>
      <th>CoralReefs</th>
      <th>DescentofMan</th>
      <th>DifferentFormsofFlowers</th>
      <th>EffectsCrossSelfFertilization</th>
      <th>ExpressionofEmotionManAnimals</th>
      <th>FormationVegetableMould</th>
      <th>FoundationsOriginofSpecies</th>
      <th>GeologicalObservationsSouthAmerica</th>
      <th>InsectivorousPlants</th>
      <th>LifeandLettersVol1</th>
      <th>LifeandLettersVol2</th>
      <th>MonographCirripedia</th>
      <th>MonographCirripediaVol2</th>
      <th>MovementClimbingPlants</th>
      <th>OriginofSpecies</th>
      <th>PowerMovementPlants</th>
      <th>VariationPlantsAnimalsDomestication</th>
      <th>VolcanicIslands</th>
      <th>VoyageBeagle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Autobiography</th>
      <td>1.000000</td>
      <td>0.049467</td>
      <td>0.080428</td>
      <td>0.066482</td>
      <td>0.077184</td>
      <td>0.088723</td>
      <td>0.040678</td>
      <td>0.059271</td>
      <td>0.030562</td>
      <td>0.014878</td>
      <td>0.396709</td>
      <td>0.217129</td>
      <td>0.005686</td>
      <td>0.008483</td>
      <td>0.022856</td>
      <td>0.099991</td>
      <td>0.016247</td>
      <td>0.049018</td>
      <td>0.038556</td>
      <td>0.183507</td>
    </tr>
    <tr>
      <th>CoralReefs</th>
      <td>0.049467</td>
      <td>1.000000</td>
      <td>0.009480</td>
      <td>0.001952</td>
      <td>0.001923</td>
      <td>0.004999</td>
      <td>0.029432</td>
      <td>0.022096</td>
      <td>0.061027</td>
      <td>0.002276</td>
      <td>0.030965</td>
      <td>0.017558</td>
      <td>0.006324</td>
      <td>0.010579</td>
      <td>0.001518</td>
      <td>0.039089</td>
      <td>0.002707</td>
      <td>0.011586</td>
      <td>0.057514</td>
      <td>0.267749</td>
    </tr>
    <tr>
      <th>DescentofMan</th>
      <td>0.080428</td>
      <td>0.009480</td>
      <td>1.000000</td>
      <td>0.072761</td>
      <td>0.029968</td>
      <td>0.148670</td>
      <td>0.027055</td>
      <td>0.135775</td>
      <td>0.009698</td>
      <td>0.009404</td>
      <td>0.059684</td>
      <td>0.080314</td>
      <td>0.053506</td>
      <td>0.043275</td>
      <td>0.005146</td>
      <td>0.267554</td>
      <td>0.011357</td>
      <td>0.232841</td>
      <td>0.007882</td>
      <td>0.123917</td>
    </tr>
    <tr>
      <th>DifferentFormsofFlowers</th>
      <td>0.066482</td>
      <td>0.001952</td>
      <td>0.072761</td>
      <td>1.000000</td>
      <td>0.391834</td>
      <td>0.006474</td>
      <td>0.010585</td>
      <td>0.040104</td>
      <td>0.002846</td>
      <td>0.007502</td>
      <td>0.015933</td>
      <td>0.046523</td>
      <td>0.009405</td>
      <td>0.005484</td>
      <td>0.008151</td>
      <td>0.128909</td>
      <td>0.018964</td>
      <td>0.050023</td>
      <td>0.002611</td>
      <td>0.013124</td>
    </tr>
    <tr>
      <th>EffectsCrossSelfFertilization</th>
      <td>0.077184</td>
      <td>0.001923</td>
      <td>0.029968</td>
      <td>0.391834</td>
      <td>1.000000</td>
      <td>0.006844</td>
      <td>0.032262</td>
      <td>0.040288</td>
      <td>0.002246</td>
      <td>0.006777</td>
      <td>0.019504</td>
      <td>0.046504</td>
      <td>0.003212</td>
      <td>0.002962</td>
      <td>0.014932</td>
      <td>0.146441</td>
      <td>0.039770</td>
      <td>0.055132</td>
      <td>0.002178</td>
      <td>0.017140</td>
    </tr>
    <tr>
      <th>ExpressionofEmotionManAnimals</th>
      <td>0.088723</td>
      <td>0.004999</td>
      <td>0.148670</td>
      <td>0.006474</td>
      <td>0.006844</td>
      <td>1.000000</td>
      <td>0.020985</td>
      <td>0.047202</td>
      <td>0.005217</td>
      <td>0.011475</td>
      <td>0.064873</td>
      <td>0.048886</td>
      <td>0.016825</td>
      <td>0.029897</td>
      <td>0.005913</td>
      <td>0.062979</td>
      <td>0.011317</td>
      <td>0.083847</td>
      <td>0.005561</td>
      <td>0.098961</td>
    </tr>
    <tr>
      <th>FormationVegetableMould</th>
      <td>0.040678</td>
      <td>0.029432</td>
      <td>0.027055</td>
      <td>0.010585</td>
      <td>0.032262</td>
      <td>0.020985</td>
      <td>1.000000</td>
      <td>0.021470</td>
      <td>0.067989</td>
      <td>0.035589</td>
      <td>0.027916</td>
      <td>0.023620</td>
      <td>0.019866</td>
      <td>0.023984</td>
      <td>0.038820</td>
      <td>0.049259</td>
      <td>0.040182</td>
      <td>0.033147</td>
      <td>0.059407</td>
      <td>0.097908</td>
    </tr>
    <tr>
      <th>FoundationsOriginofSpecies</th>
      <td>0.059271</td>
      <td>0.022096</td>
      <td>0.135775</td>
      <td>0.040104</td>
      <td>0.040288</td>
      <td>0.047202</td>
      <td>0.021470</td>
      <td>1.000000</td>
      <td>0.028028</td>
      <td>0.006023</td>
      <td>0.057820</td>
      <td>0.054782</td>
      <td>0.007618</td>
      <td>0.010883</td>
      <td>0.003973</td>
      <td>0.322405</td>
      <td>0.008788</td>
      <td>0.194533</td>
      <td>0.017590</td>
      <td>0.089132</td>
    </tr>
    <tr>
      <th>GeologicalObservationsSouthAmerica</th>
      <td>0.030562</td>
      <td>0.061027</td>
      <td>0.009698</td>
      <td>0.002846</td>
      <td>0.002246</td>
      <td>0.005217</td>
      <td>0.067989</td>
      <td>0.028028</td>
      <td>1.000000</td>
      <td>0.006879</td>
      <td>0.028551</td>
      <td>0.012104</td>
      <td>0.009687</td>
      <td>0.024738</td>
      <td>0.002043</td>
      <td>0.058046</td>
      <td>0.003491</td>
      <td>0.014389</td>
      <td>0.373249</td>
      <td>0.260141</td>
    </tr>
    <tr>
      <th>InsectivorousPlants</th>
      <td>0.014878</td>
      <td>0.002276</td>
      <td>0.009404</td>
      <td>0.007502</td>
      <td>0.006777</td>
      <td>0.011475</td>
      <td>0.035589</td>
      <td>0.006023</td>
      <td>0.006879</td>
      <td>1.000000</td>
      <td>0.005967</td>
      <td>0.016518</td>
      <td>0.019214</td>
      <td>0.020023</td>
      <td>0.249814</td>
      <td>0.014961</td>
      <td>0.023056</td>
      <td>0.010522</td>
      <td>0.008544</td>
      <td>0.014776</td>
    </tr>
    <tr>
      <th>LifeandLettersVol1</th>
      <td>0.396709</td>
      <td>0.030965</td>
      <td>0.059684</td>
      <td>0.015933</td>
      <td>0.019504</td>
      <td>0.064873</td>
      <td>0.027916</td>
      <td>0.057820</td>
      <td>0.028551</td>
      <td>0.005967</td>
      <td>1.000000</td>
      <td>0.885828</td>
      <td>0.005752</td>
      <td>0.012772</td>
      <td>0.005388</td>
      <td>0.097457</td>
      <td>0.009505</td>
      <td>0.055259</td>
      <td>0.026374</td>
      <td>0.171708</td>
    </tr>
    <tr>
      <th>LifeandLettersVol2</th>
      <td>0.217129</td>
      <td>0.017558</td>
      <td>0.080314</td>
      <td>0.046523</td>
      <td>0.046504</td>
      <td>0.048886</td>
      <td>0.023620</td>
      <td>0.054782</td>
      <td>0.012104</td>
      <td>0.016518</td>
      <td>0.885828</td>
      <td>1.000000</td>
      <td>0.004967</td>
      <td>0.010843</td>
      <td>0.017565</td>
      <td>0.096955</td>
      <td>0.012099</td>
      <td>0.050764</td>
      <td>0.011806</td>
      <td>0.089947</td>
    </tr>
    <tr>
      <th>MonographCirripedia</th>
      <td>0.005686</td>
      <td>0.006324</td>
      <td>0.053506</td>
      <td>0.009405</td>
      <td>0.003212</td>
      <td>0.016825</td>
      <td>0.019866</td>
      <td>0.007618</td>
      <td>0.009687</td>
      <td>0.019214</td>
      <td>0.005752</td>
      <td>0.004967</td>
      <td>1.000001</td>
      <td>0.522273</td>
      <td>0.012441</td>
      <td>0.029902</td>
      <td>0.018694</td>
      <td>0.023460</td>
      <td>0.010754</td>
      <td>0.014342</td>
    </tr>
    <tr>
      <th>MonographCirripediaVol2</th>
      <td>0.008483</td>
      <td>0.010579</td>
      <td>0.043275</td>
      <td>0.005484</td>
      <td>0.002962</td>
      <td>0.029897</td>
      <td>0.023984</td>
      <td>0.010883</td>
      <td>0.024738</td>
      <td>0.020023</td>
      <td>0.012772</td>
      <td>0.010843</td>
      <td>0.522273</td>
      <td>1.000000</td>
      <td>0.006802</td>
      <td>0.036755</td>
      <td>0.022376</td>
      <td>0.030669</td>
      <td>0.017952</td>
      <td>0.025047</td>
    </tr>
    <tr>
      <th>MovementClimbingPlants</th>
      <td>0.022856</td>
      <td>0.001518</td>
      <td>0.005146</td>
      <td>0.008151</td>
      <td>0.014932</td>
      <td>0.005913</td>
      <td>0.038820</td>
      <td>0.003973</td>
      <td>0.002043</td>
      <td>0.249814</td>
      <td>0.005388</td>
      <td>0.017565</td>
      <td>0.012441</td>
      <td>0.006802</td>
      <td>1.000000</td>
      <td>0.008802</td>
      <td>0.104966</td>
      <td>0.011530</td>
      <td>0.002832</td>
      <td>0.012282</td>
    </tr>
    <tr>
      <th>OriginofSpecies</th>
      <td>0.099991</td>
      <td>0.039089</td>
      <td>0.267554</td>
      <td>0.128909</td>
      <td>0.146441</td>
      <td>0.062979</td>
      <td>0.049259</td>
      <td>0.322405</td>
      <td>0.058046</td>
      <td>0.014961</td>
      <td>0.097457</td>
      <td>0.096955</td>
      <td>0.029902</td>
      <td>0.036755</td>
      <td>0.008802</td>
      <td>1.000000</td>
      <td>0.018266</td>
      <td>0.405333</td>
      <td>0.036014</td>
      <td>0.164661</td>
    </tr>
    <tr>
      <th>PowerMovementPlants</th>
      <td>0.016247</td>
      <td>0.002707</td>
      <td>0.011357</td>
      <td>0.018964</td>
      <td>0.039770</td>
      <td>0.011317</td>
      <td>0.040182</td>
      <td>0.008788</td>
      <td>0.003491</td>
      <td>0.023056</td>
      <td>0.009505</td>
      <td>0.012099</td>
      <td>0.018694</td>
      <td>0.022376</td>
      <td>0.104966</td>
      <td>0.018266</td>
      <td>1.000000</td>
      <td>0.020589</td>
      <td>0.003819</td>
      <td>0.024149</td>
    </tr>
    <tr>
      <th>VariationPlantsAnimalsDomestication</th>
      <td>0.049018</td>
      <td>0.011586</td>
      <td>0.232841</td>
      <td>0.050023</td>
      <td>0.055132</td>
      <td>0.083847</td>
      <td>0.033147</td>
      <td>0.194533</td>
      <td>0.014389</td>
      <td>0.010522</td>
      <td>0.055259</td>
      <td>0.050764</td>
      <td>0.023460</td>
      <td>0.030669</td>
      <td>0.011530</td>
      <td>0.405333</td>
      <td>0.020589</td>
      <td>1.000000</td>
      <td>0.012620</td>
      <td>0.114134</td>
    </tr>
    <tr>
      <th>VolcanicIslands</th>
      <td>0.038556</td>
      <td>0.057514</td>
      <td>0.007882</td>
      <td>0.002611</td>
      <td>0.002178</td>
      <td>0.005561</td>
      <td>0.059407</td>
      <td>0.017590</td>
      <td>0.373249</td>
      <td>0.008544</td>
      <td>0.026374</td>
      <td>0.011806</td>
      <td>0.010754</td>
      <td>0.017952</td>
      <td>0.002832</td>
      <td>0.036014</td>
      <td>0.003819</td>
      <td>0.012620</td>
      <td>1.000000</td>
      <td>0.138323</td>
    </tr>
    <tr>
      <th>VoyageBeagle</th>
      <td>0.183507</td>
      <td>0.267749</td>
      <td>0.123917</td>
      <td>0.013124</td>
      <td>0.017140</td>
      <td>0.098961</td>
      <td>0.097908</td>
      <td>0.089132</td>
      <td>0.260141</td>
      <td>0.014776</td>
      <td>0.171708</td>
      <td>0.089947</td>
      <td>0.014342</td>
      <td>0.025047</td>
      <td>0.012282</td>
      <td>0.164661</td>
      <td>0.024149</td>
      <td>0.114134</td>
      <td>0.138323</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



## 11. The book most similar to "On the Origin of Species"
<p>We now have a matrix containing all the similarity measures between any pair of books from Charles Darwin! We can now use this matrix to quickly extract the information we need, i.e., the distance between one book and one or several others. </p>
<p>As a first step, we will display which books are the most similar to "<em>On the Origin of Species</em>," more specifically we will produce a bar chart showing all books ranked by how similar they are to Darwin's landmark work.</p>


```python
# This is needed to display plots in a notebook
%matplotlib inline

# Import libraries
import matplotlib.pyplot as plt

# Select the column corresponding to "On the Origin of Species" and 
v = sim_df.iloc[:,ori]

# Sort by ascending scores
v_sorted = v.sort_values()
v_sorted

# Plot this data has a horizontal bar plot
v_sorted.plot(kind = 'barh')

# Modify the axes labels and plot title for a better readability
plt.xlabel('Score')
plt.ylabel('Book')
plt.title("Similarity with book \'On the Origin of Species\'")

```




    <matplotlib.text.Text at 0x7f65c9dbdfd0>

![png](https://user-images.githubusercontent.com/20341930/208644817-ccb3e48a-6673-4f5e-86fa-7e4a35f20a5f.png)
    


## 12. Which books have similar content?
<p>This turns out to be extremely useful if we want to determine a given book's most similar work. For example, we have just seen that if you enjoyed "<em>On the Origin of Species</em>," you can read books discussing similar concepts such as "<em>The Variation of Animals and Plants under Domestication</em>" or "<em>The Descent of Man, and Selection in Relation to Sex</em>." If you are familiar with Darwin's work, these suggestions will likely seem natural to you. Indeed, <em>On the Origin of Species</em> has a whole chapter about domestication and <em>The Descent of Man, and Selection in Relation to Sex</em> applies the theory of natural selection to human evolution. Hence, the results make sense.</p>
<p>However, we now want to have a better understanding of the big picture and see how Darwin's books are generally related to each other (in terms of topics discussed). To this purpose, we will represent the whole similarity matrix as a dendrogram, which is a standard tool to display such data. <strong>This last approach will display all the information about book similarities at once.</strong> For example, we can find a book's closest relative but, also, we can visualize which groups of books have similar topics (e.g., the cluster about Charles Darwin personal life with his autobiography and letters). If you are familiar with Darwin's bibliography, the results should not surprise you too much, which indicates the method gives good results. Otherwise, next time you read one of the author's book, you will know which other books to read next in order to learn more about the topics it addressed.</p>


```python
# Import libraries
from scipy.cluster import hierarchy

# Compute the clusters from the similarity matrix,
# using the Ward variance minimization algorithm
Z = hierarchy.linkage(sim_df.values, method = 'ward')

# Display this result as a horizontal dendrogram
plot = hierarchy.dendrogram(Z,leaf_font_size=8, labels=sim_df.index, orientation="left")
```

![png](https://user-images.githubusercontent.com/20341930/208644829-9f814d50-43ac-4f76-91d2-bb7a80e9ba85.png)

Dataset is taken from [DataCamp](https://www.datacamp.com/)