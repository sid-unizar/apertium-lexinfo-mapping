
# Apertium POS differences 
25.08.20
@(Prêt-à-LLOD)
Julia Bosque-Gil
University of Zaragoza
jbosque@unizar.es
Thanks to Shashwat Goel's work on identifying pairs with different POS and pointing out corrections in the mapping table. 

> CC -- Comments by Christian Chiarcos, 2020-08-31
> 
>JBG -- Comments by Julia Bosque-Gil, 2020-09-21

Building on [Christian Chiarcos' table in GitHub](https://github.com/acoli-repo/acoli-dicts/issues/9), I have taken those paris of Apertium RDF entries with different POS tags and compared them in source data > intermediate RDF > Apertium list of tags + mapping table > SPARQL endpoint to locate the source of the difference.   

I see that most differences are due to...
a) ...a wrong mapping of the apertium tags `apertium:nn` and `apertium:pron`. The mapping for subclasses of proper nouns was too broad as well. [Mapping table](https://github.com/sid-unizar/apertium-lexinfo-mapping) corrected now with [Shashwat's hints](https://github.com/sid-unizar/apertium-lexinfo-mapping/issues/1) and the [Apertium documentation](https://wiki.apertium.org/wiki/List_of_symbols). 
b) ...entries that show more than one POS in the source data, which is reflected in the RDF .
c) ...a combination of a) and b). 

- relativePronoun-noun and pronoun-noun mismatches. Some of these are entries which were originally tagged with POS tags (`apertium:rel` or `apertium:prn`),  and they had also other morphosyntactic tags  such as `apertium:nn` . The labels of  `apertium:nn` were

```
apertium:nn rdfs:label "Inanimate" .
apertium:nn rdfs:label "Noun noun" .
apertium:nn rdfs:label "inanimate" .
apertium:nn rdfs:label "Неодушевленное" ."
```
, which led me to **wrongly** think that `:nn` was an inanimate NOUN, so that tag is wrongly mapped to `lexinfo:noun` as POS (in addition to `lexinfo:animacy lexinfo:inanimate`).  This affects multiple (relative) pronouns that are inanimate and after the mapping they end up with two parts of speech. Mapping table corrected now, and only animacy is encoded by `:nn`.  **This should solve most mismatches there** (if we run everything again). 

> CC: TOCHECK: it seems the abbreviation `nn` is used differently in different Apertium sets. What language does "Noun noun" come from? If that is the case, this should be fixed in Apertium, not here.
> 
> JBG (2020-09-21). Checked. Max Ionov and I just confirmed this:  it turns out that `nn` is defined as "Noun noun" in hbs-mkd and hbs-slv dictionary pairs, but the tag is later not used in the entries. So it seems safe (for now) to keep `aperium:nn` as `lexinfo:inanimate`. TO-REPORT to the Apertium community (not sure if it is an error or indeed an intended different meaning for `nn`).    

- relativePronoun-adverb and pronoun-adverb mismatches. Some of these are coming from lines like the ones below: 
```
CA-FR:
<e> <p><l>quand<s n="rel"/><s n="adv"/></l>  <r>quan<s n="rel"/><s n="adv"/></r></p></e>

EO-EN:

<e> <p><l>kie<s n="rel"/><s n="adv"/></l>        <r>where<s n="rel"/><s n="adv"/></r></p></e>

FR-ES:

<e r="RL"><p><l>comme<s n="rel"/><s n="adv"/></l> <r>cual<s n="rel"/><s n="adv"/></r></p></e>
```
and the lexical entries _quand_ and _quan_ , or _kie_ and *where*, end up receiving both POS each. 

For the set of mismatches EN-ES (e.g. wherein-rel vs. donde-adv), and some OC-CA and OC-FR pairs, I cannot find the error in the RDF or in the source data for that pair exclusively, it all looks fine in both source data and intermediate RDF (one single POS). E.g. 

```
EN-ES
<e r="LR" a="imaxinsoftware"><p><l>wherein<s n="rel"/></l><r>donde<s n="rel"/></r></p></e>
``` 

But in the SPARQL the entry `:lexiconES/donde-rel-es` does indeed show two POS, adverb and relative pronoun (no other suspicious features). I assume for these cases there must be a line like the one of _quand_ and _quan_  *in at least one dictionary* where "donde" receives both "rel" and "adv". Further analysis needed. 

> CC: Possible source is URI generation during OntoLex conversion: As the SPARQL query returns the lexical entry, this seems to indicate that lexical entries with different POSes generate the same URI and thus get conflated.
>
> JBG (2020-09-21). Found it, there is a line in the EO-ES in which _donde_ does receive two POS tags:
>``` 
><e><p><l>kie<s n="rel"/><s n="adv"/></l><r>donde<s n="rel"/><s n="adv"/></r></p></e>
>```
> so even if in the source data of the dictionaries mentioned above (e.g. ES-ES) _donde_ only has one POS (rel), in the description of `:lexiconES/donde-rel-es`the `lexinfo:adverb` statement can come from the Spanish portion EO-ES. And then comparing the POSes in EN with the POSes of their translations raises the mismatch red flag.   

- personalPronoun and determiner mismatches. Similar issue in the source data as above, e.g. the lexical entry *en* in OC receives both tags, det (`lexinfo:determiner`) and pers (`lexinfo:personalPronoun`)
```
<e r="RL" alt="oc">
	<p>
		<l>lo<s n="det"/><s n="def"/></l>
		<r>en<s n="det"/><s n="pers"/></r>
	</p>
</e>
```
- verb - noun mismatches (compatir-verb-es  share-noun-en). These look OK in the RDF, we have `:lexiconEN/share-n-en` and `:lexiconES/share-vblex-en` on the one hand, and `:lexiconES/compartir-vblex-es` on the other.  Each has one single POS. 
-  noun-adverb mismatches. These mismatches are a combo of the cases explained above: more than one POS tag (like the *quant* example), and the `apertium:nn` tag mapped incorrectly to inanimate noun (first issue in this document):

```
<e r="RL"><p><l>combien<s n="rel"/><s n="adv"/></l><r>quant<s n="rel"/><s n="nn"/><s n="f"/><s n="pl"/></r></p></e>
```
> CC: see above
>
- verb-pronoun mismatches. Shashwat pointed out to me that the tag `apertium:pron` referred to a pronominal verb, not to a pronoun. Mapping table updated. **This should solve all errors found in this section. **
- pronoun-presentParticipleAdjective mismatches. A combination of the previous points: 
	- What is "pronoun" should be "verb" according to the updated mapping (incorrect mapping `apertium:pron` to `lexinfo:pron` instead of  to `lexinfo:verb`). So here we have verb-presentParticipleAdjective mismatches. 
	- There must be least one line in an English dictionary in which the word on the right column is tagged as both `apertium:vblex` and `apertium:pprs`. 
- noun - properNoun mismatches. In the mapping table not all subclasses of proper nouns where mapped as such, taking as starting point only the labels that we had for these tags. I recently updated the mapping with the Apertium documentation on these subclasses (ant, top, hyd, cog, org, al, pat), so most of these mismatches should disappear in the next version of the RDF. 

> CC: The label information in the Apertium namespace is generated from the dictionaries, but not all dictionaries provide label information. It is well possible, thus, that for a particular language, a particular abbreviation is used differently that the labels indicate. 
> 
> JBG (2020-09-21): If after the next RDF conversion round there are still mismatches that cannot be ascribed to an entry with more than a POS in at least a dictionary in the XML source data (as we saw with _donde_, and _quant_ above), they must be the case that you suggest. TOCHECK after next conversion.

Last updated: 21.09.20
