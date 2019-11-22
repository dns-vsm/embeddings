# DNS-VSM

Vector Space Model for DNS (for short, DNS-VSM) is a set of pre-trained vectors (a.k.a embeddings) for 40000 Internet domain names. These embeddings were built as part of the work presented in [1], [2] and [3].

Domain names in the DNS-VSM are represented by vectors where related domain names are mapped to nearby points in the high dimensional space.
The DNS-VSM was built only using information of DNS queries (trained on a similar dataset to the one used in [1]) without any other previous knowledge about the content hosted in each domain.

DNS embeddings can be useful in many engineering activities, with practical application in many areas.
Some examples include websites recommendations based on similar sites, competitive analysis, identification of fraudulent or risky sites, parental-control systems, UX improvements (based on recommendations, spell correction, etc.), click-stream analysis, representation and clustering of users navigation profiles, optimization of cache systems in recursive DNS resolvers, anomaly detection in network traffic analysis (among others).

## Before using the DNS-VSM

It's important to note that many domains could have disappeared and many others could have been created in the last years.
Also, since the DNS queries used for this project are not global (data used comes from only one ISP), then there could be some kind of bias on similar sites.
<b>For these two reasons, it's strongly recommended to use these vectors just for academic purposes and not for any production environment.</b>

## Download pre-trained vectors
The pre-trained vectors for the DNS-VSM can be downloaded from <a href='https://drive.google.com/file/d/1NWGd-pfjZpsgASoZW7YUUiJFjWzEdozq/view?usp=sharing'>here</a>.  Download and unzip the content to the <i>models</i> folder.

The folder structure should looks like:

```
-> models
    -> ft
        -> 21epoc_minn11_maxn17
        -> 21epoc_minn11_maxn17.wv.syn0_ngrams.npy
```


## Installation
This project was tested using python 3.6.4 and it requires gensim 3.1.0 (obs: newer versions of gensim may not work).

### Requirements
```
python 3.6.4
gensim 3.1.0
```

### Installation using pyenv with virtual env

First, create the virtual environment

```
pyenv virtualenv 3.6.4 dns-vsm
```

Now, activate the new environment

```
pyenv activate dns-vsm
```

Once the environment is activated, install the gensim dependency
```
pip install gensim==3.1.0
```
## Using the DNS-VSM
Open a terminal, activate the dns-vsm virtual environment and type <i>python</i> to enter to the Python's interactive mode.


The DNS-VSM uses the gensim's wrapper for FastText, so in order to use the DNS-VSM you need to import the FastText wrapper as follows:

``` python
from gensim.models.wrappers.fasttext import FastText as ft
```

Now, you can load the pre-trained vectors in this way:
``` python
dns_embeddings = ft.load('models/ft/21epoc_minn11_maxn17')
```

Finally, you can use the DNS-VSM to query for similar domains:
``` python
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

<br/>

### Semantic similarity

The following table analyzes the most similar sites to <i>subrayado.com.uy</i> (TV news).


| Domain name  | Type | Cosine distance | Observations |
| -------------| -----| ----------------| -------------|
| subrayado.com| Non existent domain | 0.961 |Same domain, but without country code ‘uy’|
| diariolarepublica.net| press, newspaper | 0.836 | Alias for larepublica.com.uy |
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

Table above gives strong evidence about the model’s capability for capturing semantic information about domain names.
Semantic similarity between domain names can be helpful in many scenarios, for example in the example above for recommending similar semantically related sites.

Other interesting use case where semantic similarity can be helpful could be for filtering adult content as part of a parental control system.
Suppose you know some content that you want to filter but not all of the them. 
In that case you can find and filter contents that are similar to some specific sites like in the following example:  
<br/>

``` python
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

### Analogical reasoning

One of the most beautiful thing about word embeddings (in particular those embeddings that were trained using pedictive shallow neural network models) is analogical reasoning. 

Analogical reasoning with word embeddings allows to apply simple arithmetic operations with the vector representations of words and find complex relationships between them. 

There is a famous example when working with word embeddings where <i>embedding("queen")</i> is approximated as <i>embedding("King")  + embedding("Woman") - embedding("Man")</i>. In other words, adding the vectors associated with the words king and woman while subtracting man is equal (or very similar) to the vector associated with queen.

This hidden linear structure of the vector space model for word embeddings can also be used in the DNS-VSM in a similar way to find complex relationships between domain names. 

For example:

<br/>

``` python
dns_embeddings.most_similar(positive=['atlantida.com.uy', 'maldonado.gub.uy'], negative=['canelones.gub.uy'], topn=3)
```

| <i>v1</i> | <i>v2</i> | <i>v3</i> | <i>v1 + v2 - v3</i>|
| ---| ---| ---| ------------|
| <b><i>atlantida.com.uy</i></b> <br/><br/> (site related to Atlantida, the main resort in Canelones city) | <b><i>maldonado.gub.uy</i></b> <br/><br/>(site for the Maldonado city government) | <b><i>canelones.gub.uy</i></b> <br/><br/>(site for the Canelones city government) | <b><i>puntaweb.com</i></b> <br/><br/><b><i>puntadeleste.com</i></b> <br/><br/>(sites related to Punta del Este, the main resort in Maldonado city)|


<br/>

Other example of analogical reasoning:

<br/>

``` python
dns_embeddings.most_similar(positive=['puntashopping.com.uy', 'montevideo.gub.uy'], negative=['maldonado.gub.uy'], topn=3)
```

| <i>v1</i> | <i>v2</i> | <i>v3</i> | <i>v1 + v2 - v3</i>|
| ---| ---| ---| ------------|
| <b><i>puntashopping.com.uy</i></b> <br/><br/> (site for a shopping center in Maldonado city) | <b><i>montevideo.gub.uy</i></b> <br/><br/>(site for the Montevideo city government) | <b><i>maldonado.gub.uy</i></b> <br/><br/>(site for the Maldonado city government) | <b><i>tiendasmontevideo.com</i></b> <br/><br/><b><i>montevideoshopping.com.uy</i></b> <br/><br/>(sites for shopping centers in Montevideo city)|

<br/>

The previous examples show 2 of the 3 domain names nearest to the resulting vector <i>v1 + v2 − v3</i>. 
Analogical reasoning could be helpful for understanding complex relationships between domain names. 

<br/>


### Support for out-of-vocabulary (OOV) domain names
The DNS-VSM was built using character n-grams between 11 and 17 characters. 
When a domain name does not have a vector representation in the DNS-VSM but shares some of its n-grams with some other domain name that is part of the DNS-VSM, then the DNS-VSM can approximate its vector representation and use it for all common operations as if it were part of the original DNS-VSM.
We can ilustrate this better through an example.

<br/>

``` python
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
| brou.com.uy | banking | 0.751 ||
| nbc.com.uy | banking | 0.749 |Domain does not exist anymore (Feb-2019)|

<p align="center">
<i>Most similar sites for an oov domain name.</i>
</p>

<br/>


Support for out-of-vocabulary (OOV) domain names, could be helpful for identifying domain names that for some reason are incorrect (like in the previous example), and also to
find the correct match for them. A domain name could be bad formed because of many reasons, for example because it was typed incorrectly with a typo
or because a harmful software shows a bad formed url intentionally (for example typosquatted domains or IDN homograph attacks) trying to deceive a user to redirect him/her to a website that looks identically to the original one but generally designed to steal user credentials, banking and credit card details (a.k.a phishing).

<br/>


## Jupyter notebook
You can check <a href='dns_embeddings.ipynb'>this jupyter notebook</a> with the code used in these examples and some others. 

<br/>


## References

If you use the DNS-VSM for any purpose, please cite the following works:

[1] W. Lopez, <a href='https://drive.google.com/file/d/1U1Opb1jT3cl-HoQGNe2umCA6OYK6rg18/view?usp=sharing'><i>"Vector representation of Internet domain names using word embedding techniques,"</i></a>
M.S. thesis, Instituto de Computación, Facultad de Ingenierı́a, Universidad de la República, Montevideo, Uruguay, 2019. (T)

[2] W. Lopez, J. Merlino and P. Rodriguez-Bocca, <a href='http://www.clei2017-46jaiio.sadio.org.ar/sites/default/files/Mem/SLIOIA/slioia-11.pdf'><i>"Extracting semantic information from Internet Domain Names using word embeddings"</i></a>,
submitted to Engineering Applications of Artificial Intelligence (ELSEVIER), 2019.

[3] W. Lopez, J. Merlino and P. Rodriguez-Bocca, <i>"Vector representation of internet domain names using a word embedding technique,"</i> 
2017 XLIII Latin American Computer Conference (CLEI), Cordoba, 2017, pp. 1-8.
