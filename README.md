# apertium-lexinfo-mapping

Mapping between Apertium's morphosyntactic tags and the LexInfo Ontology.

Source data of Apertium: https://github.com/apertium/apertium-trunk

Steps:

1. The list of tags extracted from Apertium data was taken as basis for the mapping. This extraction was performed at the Applied Computational Linguistics Group (ACoLi) at Goethe University Frankfurkt, see : https://github.com/acoli-repo/acoli-dicts/blob/master/stable/apertium

2. Individual by individual (or tag by tag), by relying on the `rdf:label`(s), a match in LexInfo was manually searched. See below for details on this. 

   > Update July 2020: manual addition of tags to this initial list of mappings, on the basis of Apertium's List of Symbols: https://wiki.apertium.org/wiki/List_of_symbols

3. This CVS/TSV does not explicitly provide equivalence relations between Apertium individuals and LexInfo individuals, but triple replacement instructions to apply to _an intermediate shallow RDF conversion_ (see Donandt, K., and Chiarcos, C. (2019)). Sometimes an Apertium individual will indeed be replaced by its LexInfo individual (if available), but this does not hold for all rows. 

Apertium individuals occur as object of `lexinfo:morphosyntacticProperty` in the intermediate RDF. The CVS provides "predicate - object" pairs for each of those Apertium tags acting as object. That is, if a row of the the CVS reads ```apertium:acc, lexinfo:case, lexinfo:accusativeCase```, this row is intended to guide this update:
```xml 
##  Intermediate RDF
?subject lexinfo:morphosyntacticProperty apertium:acc . 

##  Expected result after update
?subject lexinfo:case lexinfo:accusativeCase .    
```
Sometimes a single tag in Apertium is mapped to different ones in LexInfo (the tag "bundles" a set of features). This is indicated in the CVS just by adding different rows with the same Apertium tag and several mappings to LexInfo. For example, here we have a tag for _a third person singular possesive suffix_:

```txt
apertium:PxSg3, lexinfo:person, lexinfo:thirdPerson
apertium:PxSg3, lexinfo:number, lexinfo:singular
apertium:PxSg3, lexinfo:referentType, lexinfo:possessive
apertium:PxSg3, lexinfo:termElement, lexinfo:suffix
```

When no 1:1 mapping is available, there are three scenarios: 
   1. No documentation for the tag:  the same predicate (`lexinfo:morphosyntacticProperty`) and the same individual (the Apertium individual) are left unchanged. This is the reason why some rows have the same value in the first and third column (`apertium:ADB, lexinfo:morphosyntacticProperty, apertium:ADB`) in the CVS. 
   > **INPUT NEEDED** to map those tags!
   2. There is a mismatch in granularity with the potential LexInfo individual: for example, `apertium:Comp` is  a very specific comparative, and in LexInfo we have `lexinfo:comparative`, with broader semantics. To preserve the specific comparative information without losing the link to LexInfo, we keep the `apertium:Comp` value in a triple, and add the LexInfo `lexinfo:comparative` individual as well. 
   3.  Mapping  the tag to LexInfo would require to turn to a module of OntoLex-_lemon_ to properly introduce the link. This only happens with syntactic frames in this list of tags. 


**For each POS tag**, the following table provides ...

1. The lexinfo individual(s) that are instantiated throughout the data to encode that pos 
2. The list of Apertium tags (varying in granularity) for that pos in the data
3. The suggested pos abbreviation to use in `ontolex:LexicalEntry`  URIs, based on the [Universal Dependencies tagset](https://universaldependencies.org/u/pos/).
 

| POS       | Lexinfo Individual           | Apertium tag  | UD Tag
| ------------- |:-------------:| -----:| -----:|
| adjective      | adjective, presentParticipleAdjective, | A, adj, ADJ, Der_las, pprs, short, sint | ADJ | 
| adposition     | adposition, postposition, preposition |  Adp, ADP, Po, post, pr, Pr, prep, Rabl, Racc, Rdat, Rgen, Rins, Rloc, Rnom | ADP
|adverb | adverb      |   adv, Adv, cnjadv, gna, preadv  | ADV
| proper noun      | properNoun | al, ant, cog, hyd, np, org, Org, pat, Prop, top | PROPN  
| punctuation    | punctuation, comma, openParenthesis, questionMark, closeParenthesis  |  apos, clb, CLB, cm, comma, dash, guio, lpar, lquest, lquot, percent, punct, quot, quote, rpar, rquot, sent | PUNCT
| determiner     | article, determiner, demonstrativeDeterminer | art, det, DET, detnt, dst, predet, prx | DET
| conjunction    | conjunction | cnj, conj | CONJ
| conjunction    | coordinatingConjunction | CC, cnjcoo | CCONJ
| conjunction    | subordinatingConjunction | cs, CS, cnjadv, cnjsub | SCONJ
| noun   | noun | agnt, Der_eapmi, Der_muš, Der_vuohta, G3, n, N,Plc, subs | NOUN
| verb    | copula, verb | cop, pron, Pron, sep, v, V, vbavea, vbdo, vbhaver, vblex, vbloc, vbser, VGen | VERB
| auxiliary verb | verb, modal | vaux, vbmod | AUX 
| particle   | particle | emph, mod, MOD, mod_ass, mod_ind, Pcle, qst, Qst, vpart | PART
| interjection   | interjection | ij, interj  | INTJ
| numeral   | numeral | num, Num  | NUM
| pronoun    | indefinitePronoun, interrogativePronoun,  personalPronoun, pronoun, possessivePronoun, reflexivePersonalPronoun, relativePronoun, reciprocalPronoun, demonstrativePronoun| def, dem, Dem, indef, itg, pers, Pers, pos, prn, qnt, ref, rel, res | PRON


**Contributors**:
 
Julia Bosque-Gil (University of Zaragoza): Mapping Apertium-Lexinfo

Christian Chiarcos, Maxim Ionov (Goethe Universität Frankfurt): Extraction of tags from Apertium data 
https://github.com/acoli-repo/acoli-dicts/blob/master/stable/apertium/apertium.ttl

Original set of tags used in the Apertium family of dictionaries: https://github.com/apertium/apertium-trunk

**References**: 

Gracia, J., Villegas, M., Gomez-Perez, A., & Bel, N. (2018). The apertium bilingual dictionaries on the web of data. Semantic Web, 9(2), 231-240.

Donandt, K., & Chiarcos, C. (2019). Translation inference through multi-lingual word embedding similarity. In Proc. of TIAD-2019 Shared Task Translation Inference Across Dictionaries, at 2nd Language Data and Knowledge (LDK) conference. CEUR-WS.

---

**Changelog**

Last updated by jubosgil on _2020-12-02_. 

_2020-12-02_

+ Change column names in the csv and tsv files (to shorter, cleaner ones)
+ Correct UD tags to uppercase and remove superfluous spaces
+ Add analysis of the multiple labels for a given Apertium tag. The goal was to detect whether a single tag was used with a different meaning in at least one dictionary pair. See the summary table [here](https://github.com/sid-unizar/apertium-lexinfo-mapping/blob/master/docs/Analysis_of_multiple_labels_per_Apertium_tag.xlsx).
+ Remove mappings from the tags `apertium:mf` and `apertium:mfn` (differences across languages). Now they are only introduced with a `lexinfo:gender` property but *not* mapped to `lexinfo:masculine` or `lexinfo:feminine` individuals. See the table mentioned above for details.
+ Update `Readme.md` and POS mapping summary table. 

_2020-08_ and _2020-09_

+ Further analysis of the source of POS mismatches (see notes from past months) and curation of the mapping (e.g. `apertium:nn` does not always refer to a noun despite the label "Noun noun" in hbs-mkd and hbs-slv dictionary pairs) 

_2020-07_ 
 + Manual addition of missing tags to this initial list of mappings on the basis of Apertium's List of Symbols: https://wiki.apertium.org/wiki/List_of_symbols
 + Some mappings are not included in the Apertium documentation, but now mappings listed there should be included here.
 + Analysis of POS differences source-target entries. See discussion [here](https://github.com/acoli-repo/acoli-dicts/issues/9), extended with the analysis [here](https://github.com/sid-unizar/apertium-lexinfo-mapping/blob/master/docs/Apertium%20POS%20differences.md)
 + Curation of the mapping after Issue 1 and the analysis mentioned above 
 
_2020-05-21_ 

Added POS mapping summary table, including UD tag, to `README.md` 
