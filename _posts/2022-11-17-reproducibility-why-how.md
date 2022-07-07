---
layout: distill
title: "Reproducibility: on why and how"
description: Discussing the importance of reproducibility and how to address it
date: 2022-11-17

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
  - name: The Sources of Irreproducibility
  - name: Solving Reproducibility
  - name: Beyond Reproducibility
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

**NOTE:**
This is an early draft and will be updated in the future. 


## Introduction

<!-- The complexity and the focus on novelty has pushed away the inclusion of all the technical details required in scientific publications. Often times, researchers will have to omit crucial parts of their work to apeaze the reviewers. The side effect of this change is that the source code is increasingly more important. The issue even with a released source code, it is quite hard to achieve the same level of the performance that is reported in the paper. 
 -->

Let's go over a scenario. You are just starting your research, trying to select a topic. You come across some interesting paper and you try to implement it yourself. Everything seems to be worse than what it is being reported. Lucky you, the source code is already available on GitHub. After spending hours trying to figure out what is going on in someone else's code, you figure out a few steps that haven't been reported. It is hard to figure out which one is causing the disparity. You try to follow the instructions, again, you run into several issues ranging from missing files and dependency hell. After spending hours, or even days, you finally get the code to run, but still it is worse than the numbers reported. 

Now, if you think that is not fun, we are on the same page. What is surprizing is that researchers have equally a hard time reproducing their own work. A time that would have been better spend on coming out with new cool ideas has to be spend on debugging what is causing the issue in the first place. 

With increased reliance on empirical evidence, the foundation of today's research has become the most fragile it has ever been. Reproducibility has been recognized as one of the desirable characteristics of science by Open Science Initiative of United Nations Educational, Scientific and Cultural Organization (UNESCO). Let's go over what are some potential causes of irreproducibility.


## The Sources of Irreproducibility

There doesn't seem to be a known list of potential causes of irreproducibility (yet ;) ), let's try to list them here. I could thing of three potential causes:

### **Human Error**
Missing files, missing scripts, typos, local variables that are not pushed to the repository, etc. This category is the messiest, yet it is one of the easiest to resolve. It requires a bit of communication and collaboration between the authors and the reviewers. 
### **Behavior Changes in the Libraries Used**

Research is a fast-paced field, and libraries are constantly being updated. This is a double-edged sword, on one hand, it is good to have the latest and greatest, on the other hand, it can cause the code to break. This is a hard problem to solve, and it is not clear what is the best way to deal with it. Sometimes, you can't even figure out which version of the library the authors were using. You will have to try different versions and see which one works!!! Or, you could end up in a dependency hell. I can tell you from personal experience that name is not an exaggeration. It is a nightmare to deal with. This is a well-known problem in the software development community. It is not uncommon to see a project that is not maintained anymore, and it is hard to find the dependencies that are needed to run the code. This is a problem that is not limited to the research community. It is a problem that is shared by the software development community. You could even end up code that was used for older generation GPUs, and you are trying to run it on a newer generation GPU. That is a whole different story.
### **OTHER?** 

Unfortunately, we are not done yet. Did you know that besides hyperparameters, architecture, and model's initial weights, there are other factors that can affect the results? For example, the order of the data points in the training set. In a recent study, I found that random seed had higher impact on the performance than all the changes that the authors were proposing to improve the performance. This is the most challenging problem to solve. Some suggest using statistical significance analysis (e.g. bootstrap test). I tried it, it didn't work. 

 
We will discuss potential solutions in the next section.

<!-- While they are the most boring kind, often times, syntax errors are often easiest to fix. Things get slightly more complicated when it comes to   -->

## Solving Reproducibility

Yeah, that is not going to happen anytime soon. I would argue we can't even completely eliminate the first issue, the human error, but we can find and fix it with a different approach than what we are currently doing.

Part of the problem is an engineering issue, and requires all of us to come together as a community; we need to hold ourselves to a higher standard. 

We have to remember, engineering has become a part of our research these days, when we release the source code, we are not doing a world a favor, that is part of our job. Especially scientific publications that rely on empirical evidence, if there is no empirical data, then what is left? 

Ultimately, using self-contained artifacts would resolve most of category 1 and 2 issues. 

We don't know a ton about category 3 issues, but that is part of my research proposal. It seems like the only solution is to report variance without eliminating bias. 

Once all is set and done, we should be on the same page as the authors, meaning, both results should be within a an acceptable margin of error. Horray, we're done, right?

## Beyond Reproducibility

Not really, we did all that in order to be able to investigate the claims. At this point, if we're are following every single step and still not achieving the same results, either the authors or us are making a mistake. The hardest bugs to identify are irreproducible ones. This is the point we can dig deeper and investigate other causes. In a recent work, I found 3 major bugs affecting a 5 year old publication with more than 300 citations. This leads me to believe the presence of bugs are more common than we think, especially since a buggy network would still work.
