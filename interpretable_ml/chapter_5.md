## Section 5.1 - 5.7, 5.9
Section 5.1 - 5.7 and 5.9 was written in Google Docs and converted to a PDF file ([chapter_5_old.pdf](/interpretable_ml/chapter_5_old.pdf)).

## [5.8 Scoped Rules (Anchors)](https://christophm.github.io/interpretable-ml-book/anchors.html)

- Anchors explains **individual predictions** of any black-box classification model by finding a decision rule (*IF-THEN*) that "anchors" the prediction sufficiently. A rule anchors a prediction if changes in other feature values do not affect the prediction.
- An anchor $A$ is formally defined as follows

![anchor_math](/interpretable_ml/resources/images/5_8_anchor_math.jpg = 300x300)