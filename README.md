# DNS-VSM
Vector Space Model for DNS (for short, DNS-VSM) is a set of pre-trained vectors (embeddings) for 40000 Internet domain names.

Domain names in the DNS-VSM are represented by vectors where related domain names are mapped to nearby points in the high dimensional space. The DNS-VSM was built only using information of DNS queries from a large uruguayan ISP (with data from 2013) without any other previous knowledge about the content hosted in each domain.

DNS embeddings can be useful in many engineering activities, with practical application in many areas. Some examples include websites recommendations based on similar sites, competitive analysis, identification of fraudulent or risky sites, parental-control systems, UX improvements (based on recommendations, spell correction, etc.), click-stream analysis, representation and clustering of users navigation profiles, optimization of cache systems in recursive DNS resolvers (among others).



Domain name
Type
Cosine distance
Observations
subrayado.com
Non existent domain
0.916
Same domain, but without country code ‘uy’
diariolarepublica.net
press, newspaper
0.836
Alias for republica.com.uy
eldiario.com.uy
press, newspaper
0.807


lr21.com.uy
press, newspaper
0.799


teledoce.com
press, tv news
0.792


elecodigital.com.uy
press, newspaper
0.774


causaabierta.com.uy
-
0.77
Domain does not exist anymore (Jan-2019)
unoticias.com.uy
press, newspaper
0.766


radiouruguay.com.uy
press, radio, newspaper
0.766


uypress.net
press, newspaper
0.742


sangregoriodepolancodigital.com.uy


press, newspaper
0.73
Domain does not exist anymore (Jan-2019)
vivomontevideo.com
-
0.71
Domain does not exist anymore (Jan-2019)

Table 13 - Most similar domain names to subrayado.com.uy (using FastText model).
