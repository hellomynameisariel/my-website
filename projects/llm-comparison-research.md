---
layout: single
title: "Large Language Model Comparative Research and Scoring System"
date: 2023-07-10
tags: [projects, research, ai, evaluation, metrics, reports]
categories: [projects]
excerpt: "I built a structured scoring system to compare 20 LLMs across 1,500 test cases, turning complex outputs into clear, data-driven insights."
permalink: /projects/llm-comparison-research.html
published: true
---

# LLM Comparative Research

Comparing the performance of LLMs at scale requires more than side-by-side intuition. It demands structure, repeatability, and real-world scoring.

In July 2023, I was tasked with independently evaluating the outputs of approximately 20 publicly available LLMs.  

Using a set of nearly 40 unique prompts — many based on a user's reported real-world chat history, including follow-up questions and multi-turn dialogues — I designed and executed a custom testing framework to assess model performance across a diverse range of topics, from casual queries to complex coding challenges.

Rather than relying on subjective impressions, I developed a structured scoring system that converted qualitative output into quantitative analysis:

- Built a structured database to house all results
- Translated nuanced, non-numerical outputs into standardized yes/no assessments
- Assigned letter grades (A–F) to model responses based on specific criteria
- Converted letter grades into numeric values for statistical averaging and comparison
- Ran over 1,500 individual test cases to produce comprehensive, data-driven results

This approach allowed direct, measurable comparisons between models. It identified strengths, weaknesses, and meaningful differences across tasks without relying on marketing claims or surface impressions.

The final deliverable was a scalable scoring system that could be reapplied to future LLM versions or prompt sets, providing a repeatable baseline for ongoing evaluation.

---

### Key Skills Applied
- Designing independent, scalable evaluation frameworks
- Converting qualitative outputs into quantitative, scorable data
- Research methodology development and execution
- Database structuring for complex multi-variable datasets
- Statistical scoring and analysis
- Large-scale test management and reporting

---

### Future Ideas
- Expanding the framework to assess hallucination rates or citation accuracy
- Building an automated pipeline for future LLM benchmark tracking
- Applying similar frameworks to multi-modal model evaluations (e.g., text + image)
