
# VisionText-CCTV: Criminal Search Using AI

**VisionText-CCTV** is a machine learning system designed to assist in searching CCTV footage for suspects using only a **textual description**. By combining computer vision and natural language processing, the system outputs aligned embeddings of text and images, enabling fast, intelligent suspect identification across large volumes of surveillance data.

## Overview

Reviewing hours of CCTV footage to locate individuals is often a slow and resource-intensive task. This project aims to reduce that burden by enabling users to input a descriptive paragraph of a suspect (e.g. “a tall man in his 30s with curly hair and a red jacket”) and receive the most likely visual matches from surveillance footage.

The core idea is to train a model that can compare an image of a person and a text description, outputting similarity scores that identify possible matches—automating and accelerating what used to take hours of manual review.

## Dataset Creation

Due to the lack of available datasets containing paired person images and detailed descriptions, we built our own dataset based on the COCO dataset.

Steps:
- **Extraction**: Used YOLOv8 to detect and extract full-body images of people from COCO images.
  - Applied a 90% confidence threshold and filtered out objects with bounding areas below 3% of the image.
- **Cleaning**: Manually filtered ~1000 cropped images to ensure quality (clarity, full-body presence, etc.).
- **Background Removal**: Applied precise segmentation masks to eliminate background information and retain only the person.
- **Text Labeling**: Used OpenAI GPT-4o Vision to generate a paragraph-long description of each person.
  - Prompt included: build, skin tone, age, hair color and style, eye color, facial features, and notable physical characteristics.
  - Excluded pose or expression.

## Model Training

We fine-tuned the CLIP (Contrastive Language–Image Pretraining) model using our synthesized dataset.

- Training approach: Contrastive learning to align image and text embeddings.
- Tools used:
  - Vision Transformer (ViT) and Text Transformer
  - Cosine similarity in a 512-dimensional embedding space
- Evaluation:
  - Used an 80/20 train-test split.
  - Metrics: Recall@1, Recall@5, Recall@10, Median Percentile Rank

**Results (after 3 epochs of training):**
- Median rank of true match: 6.5th percentile
- Recall@1: 18%
- Recall@5: 38%
- Recall@10: 54%

## Limitations and Future Work

While performance is promising, the system is not yet fully reliable to serve as an autonomous identification tool. Instead, it can significantly **accelerate manual CCTV review** by:
- Skipping over irrelevant footage with no humans.
- Sorting or prioritizing video frames containing humans that may match descriptions.

Planned improvements:
- Freeze transformer layers to train smaller models more effectively.
- Increase dataset size and diversity.
- Optimize inference speed for real-time video search.

## References

- Tagore, A., & Gupta, R. (2020). [Hierarchical re-identification using Siamese network and color histograms](https://arxiv.org/abs/2005.03293)
- Niu, M., He, W., Zhang, L., & Wang, P. (2019). [Multi-granularity image-text alignment for text-based person search](https://arxiv.org/abs/1906.09610)
- Ma, X., Luo, H., Chang, Y., Wang, Y., & Wang, Z. (2020). [Dual-path CNN with Max Gated block for text-based person search](https://arxiv.org/abs/2009.09343)
- Amouei Sheshkal, M., et al. (2020). [Improved person re-identification using lightweight CNN and transfer learning in Siamese networks](https://arxiv.org/abs/2008.09448)

---
