# Polish Language Document Layout Detection

## Introduction

The aim of the task is to advance the research concerning document layout detection in Polish. Document layout recognition involves identifying such elements as the title, section header, main text, table, footnote, image, list, etc. The input to the system consists of page images in graphic file format, while the output is the set of identified areas representing individual structural elements (rectangle coordinates) and the corresponding element category label.

A similar task for English-language documents was carried out during the ICDAR 2023 competition: https://ds4sd.github.io/icdar23-doclaynet/. The document annotation method was adopted from the DocLayNet dataset: https://github.com/DS4SD/DocLayNet 

## Dataset

The competition will provide two test datasets, as well as a development dataset with known annotations. These datasets have been created based on books, articles, and journals made available under public domain, Creative Commons, or open source licenses, as well as documents shared with the permission of copyright holders. The data is categorized based on the type of source: scientific sources (articles and books), popular sources (articles and books), leaflets, and official documents.

Each dataset folder holds the page images (`images/`) and one JSON Lines file with one line per page:

- `poleval_doclaynet_coco_dev/dev.jsonl`: the development set (337 pages), with its annotations.
- `poleval_doclaynet_coco_test_a/test_a.jsonl`: test set A (558 pages), without annotations.

Every line describes one page:

```json
{"id": "20b45f96-3093-5c43-9dc4-08c938b29f96", "image_id": 19, "file_name": "images/Etyka 2023_00.jpg", "width": 1474, "height": 2048}
```

- `id`: the page identifier your submission must use.
- `image_id`: the page's COCO image id.
- `file_name`: the page image, relative to the dataset folder.
- `width`, `height`: the coordinate frame the boxes are given in.

In `dev.jsonl` each line also carries the annotations:

- `image_order`: the same value as `image_id`, used only to order equal scores across pages the way the COCO evaluation does.
- `boxes`: a list of `{"category": "Text", "bbox": [x, y, w, h]}`, where `bbox` is in COCO `[x, y, width, height]` form.

## Annotation method

As mentioned above, both the test sets and the development set were manually annotated according to the DocLayNet guidelines. This means that COCO-style annotations were used (https://cocodataset.org/#format-data). Regions of document pages representing a single coherent element were marked and assigned one of the following categories: "Caption", "Footnote", "Formula", "List-item", "Page-footer", "Page-header", "Picture", "Section-header", "Table", "Text", "Title".

## Evaluation
Your submissions will be evaluated using the Mean Average Precision (mAP) @ Intersection-over-Union (IoU) [0.50:0.95] metric, as used in the COCO object detection competition. In detail, we will calculate the average precision for a sequence of IoU thresholds ranging from 0.50 to 0.95 with a step size of 0.05. This metric is computed for every document category in the competition-dataset. Then the mean of the average precisions on all categories is computed as the final score.

## Submission
Upload one JSON Lines (`.jsonl`) file to the evaluation platform, at most 25 MB. Give one line per page of `test_a.jsonl`, and include every page `id` exactly once:

```json
{"id": "20b45f96-3093-5c43-9dc4-08c938b29f96", "output": [{"category": "Text", "bbox": [456.0, 1747.9, 432.0, 95.3], "score": 0.93}]}
```

For a page where nothing is detected, send an empty list: `{"id": "...", "output": []}`.

Each box in `output` has exactly three keys:

- `category`: one of "Caption", "Footnote", "Formula", "List-item", "Page-footer", "Page-header", "Picture", "Section-header", "Table", "Text", "Title".
- `bbox`: `[x, y, width, height]` in the page's coordinate frame (the `width` and `height` of its line), with width and height ≥ 0.
- `score`: your confidence, a number between 0 and 1. It orders your boxes when the average precision is computed.

As in the COCO evaluation, at most the 100 highest-scoring boxes per page and category are counted.

A file that breaks these rules is rejected at upload, with the offending line numbers. An accepted file is scored straight away: the overall mAP and the AP of each category appear on your submission within seconds. The leaderboard is ranked by the overall mAP.

## Rules
It is allowed to use additional PUBLICLY available third-party data, PUBLICLY available pre-trained models, or model ensembling for performance improvement. It is forbidden to use undisclosed or commercial resources.
Winning teams will be decided based on the rank in the leaderboard, which is determined by the mAP score.
