---
layout: model
title: Posology Large
author: John Snow Labs
name: ner_posology_large_en
date: 2020-04-22
tags: [ner, en, licensed]
article_header:
type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained named entity recognition deep learning model for posology. The SparkNLP deep learning model (NerDL) is inspired by a former state of the art model for NER: Chiu & Nicols, Named Entity Recognition with Bidirectional LSTM-CNN. 

## Predicted Entities 
Dosage, Drug, Duration, Form, Frequency, Route, Strength

{:.btn-box}
[Live Demo](https://demo.johnsnowlabs.com/healthcare/NER_POSOLOGY/){:.button.button-orange}{:target="_blank"}
[Open in Colab](https://github.com/JohnSnowLabs/spark-nlp-workshop/blob/master/tutorials/Certification_Trainings/Healthcare/1.Clinical_Named_Entity_Recognition_Model.ipynb){:.button.button-orange.button-orange-trans.co.button-icon}{:target="_blank"}
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/clinical/models/ner_posology_large_en_2.4.2_2.4_1587513302751.zip){:.button.button-orange.button-orange-trans.arr.button-icon}


## How to use
Use as part of an nlp pipeline with the following stages: DocumentAssembler, SentenceDetector, Tokenizer, WordEmbeddingsModel, NerDLModel. Add the NerConverter to the end of the pipeline to convert entity tokens into full entity chunks.

<div class="tabs-box" markdown="1">

{% include programmingLanguageSelectScalaPython.html %}


```python

clinical_ner = NerDLModel.pretrained("ner_posology_large", "en", "clinical/models") \
  .setInputCols(["sentence", "token", "embeddings"]) \
  .setOutputCol("ner")

nlpPipeline = Pipeline(stages=[clinical_ner])

empty_data = spark.createDataFrame([[""]]).toDF("text")

model = nlpPipeline.fit(empty_data)

results = model.transform(data)

```

```scala

val ner = NerDLModel.pretrained("ner_posology_large", "en", "clinical/models") \
  .setInputCols(["sentence", "token", "embeddings"]) \
  .setOutputCol("ner")

val pipeline = new Pipeline().setStages(Array(ner))

val result = pipeline.fit(Seq.empty[String].toDS.toDF("text")).transform(data)


```

</div>

{:.h2_title}
## Results
The output is a dataframe with a sentence per row and a "ner" column containing all of the entity labels in the sentence, entity character indices, and other metadata. To get only the tokens and entity labels, without the metadata, select "token.result" and "ner.result" from your output dataframe or add the "Finisher" to the end of your pipeline.

![image](/assets/images/ner_posology.png)

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|ner_posology_large_en_2.4.2_2.4|
|Type:|ner|
|Compatibility:|Spark NLP 2.4.2|
|Edition:|Official|
|License:|Licensed|
|Input Labels:|[sentence,token, embeddings]|
|Output Labels:|[ner]|
|Language:|[en]|
|Case sensitive:|false|

{:.h2_title}
## Data Source
Trained on the 2018 i2b2 dataset and FDA Drug datasets with 'embeddings_clinical'.
https://open.fda.gov/
