# DNS-VSM

Vector Space Model for DNS (for short, DNS-VSM) is a set of pre-trained vectors (embeddings) for 40000 Internet domain names. These embeddings were built as part of the work presented in [1], [2] and [3].

Domain names in the DNS-VSM are represented by vectors where related domain names are mapped to nearby points in the high dimensional space. The DNS-VSM was built only using information of DNS queries from a large uruguayan ISP (with data from 2013) without any other previous knowledge about the content hosted in each domain. 

DNS embeddings can be useful in many engineering activities, with practical application in many areas. Some examples include websites recommendations based on similar sites, competitive analysis, identification of fraudulent or risky sites, parental-control systems, UX improvements (based on recommendations, spell correction, etc.), click-stream analysis, representation and clustering of users navigation profiles, optimization of cache systems in recursive DNS resolvers (among others).

## Before using the DNS-VSM

It's important to note that these vectors are from 2013 and since then many domains have disappeared and many others have been created. Also, since the DNS queries used for this project are not global (data used comes just from one ISP located in Uruguay), then simlar sites can be bias to uruguayan sites. <b>For these two reasons, it's strongly recommended to use these vectors just for academic purposes and not for any production enviroment.</b>

## Download pre-trained vectors
The pre-trained vectors for the DNS-VSM can be download from <a href='#'>here</a>. 
Download and unzip the content to the <i>models</i> folder.   

## Installation
This project was tested using python 3.6.4 and it requires gensim 3.1.0 (obs: newer versions of gensim may not work).

### Requirements
```
python 3.6.4
gensim 3.1.0
```

### Installation using pyenv with virtual env

First, create the virtual enviroment

```
pyenv virtualenv 3.6.4 dns-vsm
```

Now, activate the new enviroment

```
pyenv activate dns-vsm
```

Once the enviroment is activated, install the gensim dependecy
```
pip install gensim==3.1.0
```
### Using the DNS-VSM
Open a terminal, activate the dns-vsm virtual enviroment and type <i>python</i> to enter to the Python's interactve mode.


The DNS-VSM uses the gennsim's wrapper for FastText, so in order to use the DNS-VSM you need to import the FastText wrapper as follows:

```
from gensim.models.wrappers.fasttext import FastText as ft
```

Now, you can load the pre-trained vectors in this way:
```
dns_embeddings = ft.load('models/ft/21epoc_minn11_maxn17')
```

Finally, you can use the DNS-VSM to query for similar domains:
```
dns_embeddings.most_similar('subrayado.com.uy', topn=12)
```

You should see the following output:
```
[('subrayado.com', 0.9160100221633911),
 ('diariolarepublica.net', 0.8355216979980469),
 ('eldiario.com.uy', 0.807044267654419),
 ('lr21.com.uy', 0.7994014024734497),
 ('teledoce.com', 0.7916869521141052),
 ('elecodigital.com.uy', 0.7739754915237427),
 ('causaabierta.com.uy', 0.7702589631080627),
 ('unoticias.com.uy', 0.7664780616760254),
 ('radiouruguay.com.uy', 0.7660830020904541),
 ('uypress.net', 0.742232084274292),
 ('sangregoriodepolancodigital.com.uy', 0.7303887605667114),
 ('vivomontevideo.com', 0.710254430770874)]
```

#### Semantic similarity

The table bellow analyzes the most similar sites to <i>subrayado.com.uy</i> (TV news).


| Domain name  | Type | Cosine distance | Observations |
| -------------| -----| ----------------| -------------|
| subrayado.com| Non existent domain | 0.961 |Same domain, but without country code ‘uy’|
| diariolarepublica.net| press, newspaper | 0.836 | Alias for republica.com.uy |
| eldiario.com.uy | press, newspaper | 0.807 ||
| lr21.com.uy | press, newspaper | 0.799 ||
| teledoce.com | press, tv news | 0.792 ||
| elecodigital.com.uy | press, newspaper | 0.774 ||
| causaabierta.com.uy | - | 0.77 | Domain does not exist anymore (Jan-2019) |
| unoticias.com.uy | press, newspaper | 0.766 ||
| radiouruguay.com.uy | press, radio, newspaper | 0.766 ||
| uypress.net | press, newspaper | 0.742 ||
| sangregoriodepolancodigital.com.uy| press, newspaper | 0.73 | Domain does not exist anymore (Jan-2019) |
| vivomontevideo.com | - | 0.71 | Domain does not exist anymore (Jan-2019) |

<p align="center">
<i>Most similar sites to subrayado.com.uy (TV news site)</i>
</p>

<br/>
Table above give strong evidence about the model’s capability for capturing semantic information about domain names.
Semantic similarity between domain names can be helpful in many scenarios, for example in the example above for recommending similar semantically related sites.
Other interesting use case where semantic similarity can be helful could be for filtering adult content as part of a parental control system.
Suppose you know some content that you want to filter but not all of the them. 
In that case you can find and filter contents that are similar to some specific sites like in the following example:  
<br/>

```
dns_embeddings.most_similar('pornhub.com', topn=10)
```

| Domain name  | Type | Cosine distance |
| -------------| -----| ----------------|
| youporn.com| adult website | 0.879 |
| phncdn.com| adult website | 0.84 |
| tube8.com | adult website | 0.795 |
| youporn.com.es | adult website | 0.758 |
| videospornhub.com | adult website | 0.708 |
| xxxcupid.com | adult website | 0.696 |
| german-youporn.com | adult website | 0.696 |
| pornhubpremium.com | adult website | 0.693 |
| genericlink.com | - | 0.687 |
| youporngay.com | adult website | 0.68 |

<i>Most similar sites to pornhub.com (an adult specific content site)</i>

<br/>

#### Analogical reasoning

Other use case scenario is analogical reasoning with Internet domain names:

<br/>

```
dns_embeddings.most_similar(positive=['atlantida.com.uy', 'maldonado.gub.uy'], negative=['canelones.gub.uy'], topn=3)
```

| <i>v1</i> | <i>v2</i> | <i>v3</i> | <i>v1 + v2 - v3</i>|
| ---| ---| ---| ------------|
| <b><i>atlantida.com.uy</i></b> <br/><br/> (site related to Atlantida, the main resort in Canelones city) | <b><i>maldonado.gub.uy</i></b> <br/><br/>(site for the Maldonado city government) | <b><i>canelones.gub.uy</i></b> <br/><br/>(site for the Canelones city government) | <b><i>puntaweb.com</i></b> <br/><br/><b><i>puntadeleste.com</i></b> <br/><br/>(sites related to Punta del Este, the main resort in Maldonado city)|


<br/>

Other example of analogical reasoning:

<br/>

```
dns_embeddings.most_similar(positive=['puntashopping.com.uy', 'montevideo.gub.uy'], negative=['maldonado.gub.uy'], topn=3)
```

| <i>v1</i> | <i>v2</i> | <i>v3</i> | <i>v1 + v2 - v3</i>|
| ---| ---| ---| ------------|
| <b><i>puntashopping.com.uy</i></b> <br/><br/> (site for a shopping center in Maldonado city) | <b><i>montevideo.gub.uy</i></b> <br/><br/>(site for the Montevideo city government) | <b><i>maldonado.gub.uy</i></b> <br/><br/>(site for the Maldonado city government) | <b><i>tiendasmontevideo.com</i></b> <br/><br/><b><i>montevideoshopping.com.uy</i></b> <br/><br/>(sites for shopping centers in Montevideo city)|

<br/>

The previous examples show 2 of the 3 domain names nearest to the resulting vector <i>v1 + v2 − v3</i>. 
Analogical reasoning could be helpful for understanding complex relationships between domain names. 

<br/>


#### Support for out-of-vocabulary (OOV) domain names.
The DNS-VSM was built using character n-grams between 11 and 17 characters. 
When a domain names that does not not have a vector representation in the DNS-VSM shares some of its n-grams with some other domain name that is part of the DNS-VSM, then the DNS-VSM can approximate its vector representation and use it for all common operations as if it were part of the original DNS-VSM.
We can ilustrate this better through an example.

<br/>

```
dns_embeddings.most_similar('samtanderuniversidades.con.uy', topn=9)
```

| Domain name  | Type | Cosine distance | Observations |
| -------------| -----| ----------------| -------------|
| <b>santanderuniversidades.com.uy</b>| banking | 0.995 |<b>This is the real site</b>|
| bancamovilsantander.com.uy| banking | 0.953 ||
| santander.com.uy | banking | 0.918 ||
| multidiscount.net | banking | 0.811 ||
| bcu.gub.uy | banking | 0.808 ||
| discbank.com.uy | banking | 0.750 ||
| browserforthebetter.com | - | 0.785 | Domain does not exist anymore (Feb-2019) |
| brou.com.uy | press, newspaper | 0.751 ||
| nbc.com.uy | press, radio, newspaper | 0.749 |Domain does not exist anymore (Feb-2019)|

<p align="center">
<i>Most similar sites for an oov domain name.</i>
</p>

<br/>


Support for out-of-vocabulary (OOV) domain names, could be helpful for identifying domain names that for some reason are incorrect, and also to
find the correct match for it. A domain name could be bad formed because of many reasons, for example because it was typed incorrectly with a typo
or because a harmful software shows a bad formed url intentionally (for example typosquatted domains 63 or IDN homograph attacks 64 ) trying to deceive a user to redirect him/her to a website that looks identically to the original one but generally designed to steal user credentials, banking and credit card details (a.k.a phishing ).

<br/>


## Jupyter notebook
You can check <a href='dns_embeddings.ipynb'>this jupyter notebook</a> with the code used in these examples and some others. 

<br/>


## References

[1] W. Lopez, <i>"Vector representation of Internet domain names using word embedding techniques,"</i> 
M.S. thesis, Instituto de Computación, Facultad de Ingenierı́a, Universidad de la República, Montevideo, Uruguay, 2019.

[2] W. Lopez, J. Merlino and P. Rodriguez-Bocca, <i>"Extracting semantic information from Internet Domain Names using word embeddings"</i>,
submitted to Engineering Applications of Artificial Intelligence (ELSEVIER), 2019.

[3] W. Lopez, J. Merlino and P. Rodriguez-Bocca, <i>"Vector representation of internet domain names using a word embedding technique,"</i> 
2017 XLIII Latin American Computer Conference (CLEI), Cordoba, 2017, pp. 1-8.



