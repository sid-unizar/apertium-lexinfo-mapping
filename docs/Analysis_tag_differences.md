
**Preliminary analysis**

Last updated 2020-12-10 by jubosgil.  

Some entries show more than one POS tag across Apertium data. For example, according to the Apertium documentation, `itg` stands for an interrogative pronoun, and thus we find entries with the tags ['adj', 'itg', 'm', 'sg'], which would receive in the transformation to RDF both a `lexinfo:adjective` and a `lexinfo:pronoun` part of speech statement. We have parsed Apertium .dix files and have generated _only for those entries that show more than one tag, not necessarily POS)_ the tables named `multiple_tag_diffs.tsv` in this folder, and its version with only tags lists for column source and column target. The motivatation is to check the distribution of these multiple-POS-tag entries and the tags of their translations to prevent errors in the transformation to RDF. 

> In the following, the first list ("[...]") will always refer to source entry tags, the second to the target entry tags. 

Detected differences: 

- Interrogative adjective vs. interrogative pronoun: ['adj', 'itg', 'm', 'sg']['prn', 'itg', 'mf', 'sg']
- Relative pronoun and adverb vs. only relative: ['rel', 'adv']['rel', 'an', 'm', 'sg']
- Relative pronoun and pronoun, vs. only pronoun: ['prn', 'rel'] ['rel']
- Pronoun and relative pronoun vs. determiner and interrogative pronoun:['prn', 'rel']	['det', 'itg']
- Determiner and pronoun vs. adjective 
['det', 'qnt', 'pl']['adj', 'f', 'pl']
- Determiner vs. determiner and pronoun
['det', 'ind'] ['det', 'qnt', 'pl']	


Some detected differences affecting a single POS tag:

- Nouns with adjective translation: 

```
<e><p><l>administrasi<s n="n"/><s n="sg"/></l><r>pentadbiran<s n="adj"/></r></p></e>
```
Source: IND-ZLM

- Nouns with verb translation: 
```
<e><p><l>air<s n="n"/><s n="actio"/><s n="actv"/><s n="si"/></l><r>mengairi<s n="vblex"/></r></p></e>

[...]

<e><p><l>aborsi<s n="n"/></l><r>gugur<s n="vblex"/><s n="subst"/><s n="peN-an"/></r></p></e>  
```
Source: IND-ZLM

- Pronoun with verb translation: 
```
<e r="LR"><p><l>apakah<s n="prn"/><s n="itg"/></l><r>ada<s n="vblex"/></r></p></e>  
``` 

- Verbs with adjective translation: 
```
<e r="RL" a="hector"><p><l>bouter<s n="vblex"/><s n="pp"/></l><r>ple<g><b/>a<b/>vessar</g><s n="adj"/></r></p></e>
``` 
Source: FRA-CAT 
```
<e><p><l>biti<s n="vbser"/><s n="perf"/><s n="pprs"/><s n="adv"/></l><r>бивш<s n="adj"/><s n="m"/><s n="sg"/><s n="nom"/><s n="ind"/></r></p></e>
``` 
Source: HBS-MKD


- Determiners with noun translation: 

```
<e r="RL"><p><l>sebuah<s n="det"/><s n="ind"/></l><r>batang<s n="n"/><s n="attr"/><s n="se"/></r></p></e>
```
 
Source: IND-ZLM

- Pronoun with adverb translation: 

```
<e r="RL"><p><l>alia<s n="prn"/><s n="tn"/><s n="sg"/><s n="nom"/></l><r>else<s n="adv"/></r></p></e>
```   
Source: EO-EN

	
	