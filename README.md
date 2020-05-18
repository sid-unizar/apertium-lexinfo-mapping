# apertium-lexinfo-mapping

Mapping between Apertium's morphosyntactic tags and the LexInfo Ontology.

Source data of Apertium: https://github.com/apertium/apertium-trunk

Steps:

1. The list of tags extracted from Apertium data was taken as basis for the mapping. This extraction was performed at the Applied Computational Linguistics Group (ACoLi) at Goethe University Frankfurkt, see : https://github.com/acoli-repo/acoli-dicts/blob/master/stable/apertium

2. Individual by individual (or tag by tag), by relying on the `rdf:label`(s), a match in LexInfo was manually searched. See below for details on this. 

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



**Contributors**:
 
Julia Bosque-Gil (University of Zaragoza): Mapping Apertium-Lexinfo

Christian Chiarcos, Maxim Ionov (Goethe Universität Frankfurt): Extraction of tags from Apertium data 
https://github.com/acoli-repo/acoli-dicts/blob/master/stable/apertium/apertium.ttl

Original set of tags used in the Apertium family of dictionaries: https://github.com/apertium/apertium-trunk

**References**: 

Donandt, K., & Chiarcos, C. (2019). Translation inference through multi-lingual word embedding similarity. In Proc. of TIAD-2019 Shared Task Translation Inference Across Dictionaries, at 2nd Language Data and Knowledge (LDK) conference. CEUR-WS.


