# Polish Language Document Layout Detection

## Introduction

The aim of the task is to advance the research concerning document layout detection in Polish. Document layout recognition involves identifying such elements as the title, section header, main text, table, footnote, image, list, etc. The input to the system consists of page images in graphic file format, while the output is the set of identified areas representing individual structural elements (rectangle coordinates) and the corresponding element category label.

A similar task for English-language documents was carried out during the ICDAR 2023 competition: https://ds4sd.github.io/icdar23-doclaynet/. The document annotation method was adopted from the DocLayNet dataset: https://github.com/DS4SD/DocLayNet 

## Dataset

The competition will provide two test datasets, as well as a development dataset with known annotations. These datasets have been created based on books, articles, and journals made available under public domain, Creative Commons, or open source licenses, as well as documents shared with the permission of copyright holders. The data is categorized based on the type of source: scientific sources (articles and books), popular sources (articles and books), leaflets, and official documents.

## Annotation method

As mentioned above, both the test sets and the development set were manually annotated according to the DocLayNet guidelines. This means that COCO-style annotations were used (https://cocodataset.org/#format-data). Regions of document pages representing a single coherent element were marked and assigned one of the following categories: "Caption", "Footnote", "Formula", "List-item", "Page-footer", "Page-header", "Picture", "Section-header", "Table", "Text", "Title".

## Evaluation
Your submissions will be evaluated using the Mean Average Precision (mAP) @ Intersection-over-Union (IoU) [0.50:0.95] metric, as used in the COCO object detection competition. In detail, we will calculate the average precision for a sequence of IoU thresholds ranging from 0.50 to 0.95 with a step size of 0.05. This metric is computed for every document category in the competition-dataset. Then the mean of the average precisions on all categories is computed as the final score.

## Submission
We ask you to upload a TSV file in COCO results format (which is actually a JSON file, but the evaluation platform expects a TSV extension), with complete layout bounding-boxes for each page sample. The given image_ids must correspond to the ones we publish with the competition data-set's coco.json. For each submission you make, the computed mAP will be provided for each category as well as combined. The leaderboard will be ranked based on the overall mAP.

## Rules
It is allowed to use additional PUBLICLY available third-party data, PUBLICLY available pre-trained models, or model ensembling for performance improvement. It is forbidden to use undisclosed or commercial resources.
Winning teams will be decided based on the rank in the leaderboard, which is determined by the mAP score.
