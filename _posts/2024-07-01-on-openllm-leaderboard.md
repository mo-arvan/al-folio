---
layout: distill
title: "On OpenLLM Leaderboard"
description: Technical review of the latest changes in the OpenLLM leaderboard
date: 2024-07-01

authors:
  - name: Mohammad Arvan
    url: "https://mo-arvan.github.io/"
    affiliations:
      name: Department of Computer Science, University of Illinois at Chicago


bibliography: 2018-12-22-distill.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Introduction
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  - name: Summary of Changes
  - name: Technical Review
  # - name: Layouts
  # - name: Other Typography?

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---


## Introduction

The OpenLLM leaderboard is a collection of benchmarks for large language models. Unlike numbers reported in marketing materials, the results on the leaderboard are reproducible and can be verified by anyone. This leaderboard is managed by the Hugging Face team and is based on the Harness (lm-eval) package from EleutherAI. Overall, the leaderboard is a valuable resource for the community to compare different models and track progress in the field. Recently, the Hugging Face team announced several changes to the leaderboard. In this post, I will review these changes and discuss their implications.

## Summary of Changes

In their blog post, the Hugging Face team raise several issues with this leaderboard. Firstly, they highlight the problem of saturation, where performance on certain benchmarks reaches human level.

Moreover, they suggest that certain models are showing signs of data contamination. This is a serious issue that can lead to misleading results and conclusions. Without a clear chain of custody for the data used in training, it's difficult to confirm data contamination in a model.

Finally, they point out some of the recent investigations into issues in datasets quality, particularly MMLU-Redux and MMLU-Pro.

To address these issues, they are rebooting the leaderboard with newly released benchmarks and high-quality datasets. Additionally, they are introducing a normalization step, setting a random baseline as 0 and the maximum score as 100. They also updated the Harness (lm-eval) package from EleutherAI to its latest version and added support for LoRA finetuning. They claim these changes should enhance the reproducibility of the results.

To combat data contamination in submitted models, they are introducing a maintainer's highlight. They will handpick models released by reputable organizations that have been thoroughly vetted. This list will remain open and evolve based on community feedback and their own observations.

Lastly, they are introducing a voting mechanism for running new models on the leaderboard. This aims to address the issue of submission spamming, where multiple variants of a model are submitted to see which performs best. They hope the voting mechanism will mitigate this problem. Additionally, there are user interface improvements thanks to the work of the Gradio team.

## Technical Review

I find all these changes to be positive steps towards more reliable and reproducible benchmarks. While the maintainer's highlight section could introduce biases from the Hugging Face team, their language suggests they are open to feedback and aim to be community-driven. Given the challenges of identifying models trained on contaminated data, I believe this is a reasonable compromise. However, I still think the evaluation process could be improved.

My primary concern with this leaderboard is the lack of effort to account for stochasticity in the evaluation process. Although training neural networks involves sources of randomness such as weight initialization, data order, and dropout, once trained, these models become deterministic. They output a probability distribution for each class they are trained on, and in the case of language models, the classes are the tokens in the vocabulary. Greedy selection of the highest probability results in generated text that often appears "robotic." To address this, the common approach is to use sampling with an adjusted temperature for the softmax function, introducing randomness in the generation process. Given that two datasets, IFEval and MATH, utilize text generation, it is important to account for this variability and report a set of results for each model.

Regarding other tasks, it seems that lm-harness is moving towards a standardized template and format for all model evaluations. While this is systematic, it might backfire because in real-world use cases, inputs might not always be in the same format. This could result in models that are not robust to different inputs. Therefore, I believe evaluating the models with various input formats and reporting a set of results would provide a more accurate picture of the models' performance.

Both of these changes would indeed necessitate additional computational resources and time. It's possible that lm-eval already addresses such variability, although the blog post does not mention it explicitly. In my next post, I will delve deeper into the source code of lm-eval and see if these concerns are already addressed.