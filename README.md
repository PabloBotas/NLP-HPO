# NLP-HPO-datasets
Repository aggregating HPO-labelled datasets for NLP development.

## Table of Contents
[HPO Gold Standardized Corpora](#GSC)

[HPO Gold Standardized Corpora +](#GSC+) 

<a name="GSC"/>

## HPO Gold Standardized Corpora (GSC)
The HPO gold standard corpus consists of a collection of 228 manually annotated abstracts cited by the Online Mendelian Inheritance in Man (OMIM) database. All annotations are stored in stand-off tab based format in files carrying the PMIDs corresponding to the abstracts listed in the original corpus. The stand-off annotation format is: startOffset::endOffset [tab] HPO URI | original text span (for example [86::103] HP_0001792 | hypoplastic nails).

Groza et al. (2015, https://academic.oup.com/database/article/doi/10.1093/database/bav005/2433137) analyzed it to compare NCBO Annotator, OBO Annotator and Bio-LarK CR. Lobo et al. (2017, https://www.hindawi.com/journals/bmri/2017/8565739/) created IHP.

| | Precision | Recall | F1 | Link |
|-|-----------|--------|----|------|
| NCBO Annotator | 0.54 | 0.39 | 0.45 | https://doi.org/10.1186/1471-2105-10-S9-S14 |
| OBO Annotator  | 0.69 | 0.44 | 0.54 | https://doi.org/10.1093/database/bau045 |
| Bio-LarK CR    | 0.65 | 0.49 | 0.56 | https://doi.org/10.1093/database/bav005 |
| IHP            | 0.56 | 0.79 | 0.65 | https://doi.org/10.1155/2017/8565739 |

Link to data 1: https://data.monarchinitiative.org/hpo/hpo_corpus.tar.gz
Link to data 2: http://www.bio-lark.org/hpo_res.html

License: https://hpo.jax.org/app/license

### Inconsistencies (Groza et al. 2015)
The original GSC contains inconsistencies that could bring confusion to the machine learning-based annotator. By inconsistency we mean similar entity mentions that were annotated differently in multiple locations of a given corpus. The inconsistencies found in the GSC can be divided into four different types: number of annotations, entity meaning, nested entities, and superclass/subclass entities. Some examples of these inconsistencies are presented below.

  1. **Number of Annotations**. The number of times an entity is identified in a document is inconsistent. For example, the entity “preauricular pits” (document 998578 in the GSC) is used three times during the text and is annotated all three times. In another situation, the entity “medulloblastoma” (document 19533801 in the GSC) is also used three times during the text but it is only annotated twice.
  2. **Entity Meaning**. In some situations, annotated entities do not exactly match their meaning in the ontology, which can lead to some entities being misidentified. An example is the annotation of “calcium metabolism” (document 6882181 in the GSC) instead of “disturbance of calcium metabolism.” The entity “calcium metabolism” by itself does not have any meaning in the HPO because it does not correspond to any abnormality.
  3. **Nested Entities**. Nested entities are entities that are contained within other entities. In the GSC some of the entities that are nested inside another entity are annotated while other times they are not. An example of this occurs in the entity “skin and genital anomalies” (document 12219090 in the GSC) and “spine and rib anomalies” (document 9096761 in the GSC). The entity “spine and rib anomalies” is annotated in the GSC, along with the entity “rib anomalies.” However, the same is not true for the entity “skin and genital anomalies.” This entity is annotated in the GSC (accession number: HP_0000078) but the entity “genital anomalies,” which exists in the HPO, is not.
  4. **Superclass/Subclass Entities**. The final type of inconsistency found has to do with superclass/subclass entities. This is closely related to nested entities because it also involves the identification of entities inside other entities. It is possible that some annotators identify only the most specific class (the subclass) of a certain entity, while others try to identify all the possible classes. However, the GSC is not consistent with the annotation of these types of entities. An example of this occurs with the superclass entity “tumours” and the subclass entities “tumours of the nervous system” (document 2888021 in the GSC) and “intracranial tumours” (document 3134615 in the GSC). In the first case, the GSC annotates both “tumours” and “tumours of the nervous system” as entities. In the second case, only “intracranial tumours” is considered an entity.

<a name="GSC+"/>

## HPO Gold Standardized Corpora Extended (GSC+)

Bio-LarK CR could not be evaluated on this dataset. Bio-LarK CR -measure was only 0.02 higher than OBO Annotator so it should be ok.

| | Precision | Recall | F-measure | Link |
|-|-----------|--------|-----------|------|
| NCBO Annotator | 0.688 | 0.455 | 0.548 | https://doi.org/10.1186/1471-2105-10-S9-S14 |
| OBO Annotator | 0.769 | 0.344 | 0.475 | https://doi.org/10.1093/database/bau045 |
| MER | 0.649 | 0.405 | 0.499 | https://www.researchgate.net/profile/Francisco_Couto/publication/316545534_MER_a_Minimal_Named-Entity_Recognition_Tagger_and_Annotation_Server/links/5903169f0f7e9bc0d588d788/MER-a-Minimal-Named-Entity-Recognition-Tagger-and-Annotation-Server.pdf |
| IHP | 0.872 | 0.854 | 0.863 | https://doi.org/10.1155/2017/8565739 |

## Advantages over GSC

Link to data: https://github.com/lasigeBioTM/IHP/blob/master/GSC+.rar

IHP tries to identify all instances of HPO entities (normal, nested, and subclass/superclass entities), independently of the number of times they appear in the text. Having the correct number of times entities appear in a document can be useful for calculating important values such as the term frequency, which is used to determine the importance of a term in a document. Since IHP tries to annotate as many entities as possible, it will identify a lot of entities that are not in the GSC. This, of course, will cause a decrease in precision and therefore in the overall performance. Table 4 presents the results from the conducted test to evaluate the potential of IHP in case these inconsistencies were not an issue. Since the test removes from the results all instances of false positives that exist either in the GSC annotations or in the HPO database (by exact matching), there is an increase in precision. The results show that IHP has the potential of achieving an -measure of about 0.82, corresponding to an increase of 0.18 in comparison to the achieved results. This increase suggests that almost a fifth of the annotator’s performance can be affected by inconsistencies.

In order to determine IHP’s performance in a situation where these inconsistencies were not an issue, the GSC+ was developed in order to provide a more consistent annotation of the abstracts. The development of the GSC+ involves the addition of automatic annotations from the IHP that were matched (with exact matching) with word phrases in the HPO database and GSC annotations. The GSC+ disentangles some of GSC’s faults since it adds entities that already were considered HPO entities. Further testing may be conducted in the future to test the effects of the IHP’s feature sets and validation step on the GSC+.

The results of IHP in Table 5 were obtained using the same annotation process as with the original GSC. It achieved an -measure of 0.863, an increase of more than 0.04 to its potential performance discussed above, due to the fact that GSC+ includes more improvements than just filtering false positives. IHP had a higher performance than the other annotators on the GSC+. The results show that all three annotators have a lower recall than precision, meaning that they identify a low number of entities in comparison to the total entities in the GSC+. While these annotators all use the HPO as the target ontology to annotate the text, they most likely use exact matching to identify entities in the text and therefore are not prepared for the level of syntactical variation that occurs in HPO entities. Although, IHP is not necessarily capable of identifying entities as long as 14 words, it tries to do it by using validation rules that expand the boundaries of identified entities. Annotators that use exact matching are not able to identify these types of entities since all the words in the entity would have to exactly match the string on the HPO Ontology, which is unlikely. We did not evaluate each validation rule individually as was presented before with GSC, but we expect their impact to be highly similar in GSC+ since we are using the same corpus and ontology.

Another issue of these annotators is the choice of identifying subclass entities over superclass ones. Some annotators, like the OBO Annotator, prefer more specific annotations than more general ones and, therefore, will only identify a portion of those entities.

The reason IHP had a better performance than the other annotators on the GSC+ is that it tries to annotate all instances of HPO entities. We can also see this by comparison of the results in the GSC and the GSC+. There is an increase in precision because all the entities that were previously seen as false positives (and that exist in the HPO database) are now considered true positives.


