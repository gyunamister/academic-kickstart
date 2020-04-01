---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Business Process Intelligence"
event:
event_url:
location:
summary: "Undergraduate Course at Computer Science, RWTH-Aachen University (04.2020 ~ Ongoing)"
abstract:

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: 2020-03-15T18:29:36+01:00
date_end: 2020-03-15T18:29:36+01:00
all_day: false

# Schedule page publish date (NOT talk date).
publishDate: 2020-03-15T18:29:36+01:00

authors: []
tags: []

# Is this a featured talk? (true/false)
featured: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

# Optional filename of your slides within your talk's folder or a URL.
url_slides:

url_code:
url_pdf:
url_video:

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
Reference: [PADS website](https://www.pads.rwth-aachen.de/cms/PADS/Studium/Kurse/~qgkn/Business-Process-Intelligence/lidx/1/)

### Course Contents and Motivation

This course starts with an overview of approaches and technologies that use event data to support decision making and business process (re)design. Subsequently, the course focuses on process mining as a bridge between data mining and business process modeling. Business Process Intelligence and process mining enable engineers to understand, diagnose, improve, and streamline operational processes for a wide variety of organizations and systems as hospitals, banks, high-tech systems, governments, electronic shops, transportation systems, trading systems.

Process mining is part of the larger data science discipline. Data science aims to answer questions as:

- What really happened? (discovery)
- Why did it happen? (root cause analysis)
- What will happen? (prediction)
- What is the best that can happen? (recommendation)

Process mining is an enabling technology to answer such questions about operational processes in different domains. There is a huge demand for engineers having the skills and tools to turn event data into real value.

The course is at an introductory level with various practical assignments.

The course covers the three main types of *process mining*.

- The first type of process mining is *discovery*. A discovery technique takes an event log and produces a process model without using any a-priori information. An example is the alpha-algorithm that takes an event log and produces a Petri net explaining the behavior recorded in the log.
- The second type of process mining is *conformance*. Here, an existing process model is compared with an event log of the same process. Conformance checking can be used to check if reality, as recorded in the log, conforms to the model and vice versa.
- The third type of process mining is *enhancement*. Here, the idea is to extend or improve an existing process model using information about the actual process recorded in some event log. Whereas conformance checking measures the alignment between model and reality, this third type of process mining aims at changing or extending the a-priori model. An example is the extension of a process model with performance information, e.g., showing bottlenecks.

Process mining techniques can be used in an offline, but also online setting. The latter is known as *operational support*. An example is the detection of non-conformance at the moment the deviation actually takes place. Another example is time prediction for running cases, i.e., given a partially executed case the remaining processing time is estimated based on historic information of similar cases.

Process mining provides not only a bridge between data mining and business process management; it also helps to address the classical divide between "business" and "IT". *Evidence-based* business process management based on process mining helps to create a common ground for business process improvement and information systems development.

In recent years, process mining has become the primary data-driven BPM (Business Process Management) approach. Process mining is also increasingly applied in other domains (auditing, production, etc.). The attention for Big Data and the uptake of data science strengthen this development. Process mining is where “Data Science” and “Process Science” meet! Currently, there are about 25 software vendors offering process mining tools. Next, to Disco (Fluxicon’s tool is used in the course next to the open-source tool ProM), tools like Celonis Process Mining, ProcessGold Enterprise Platform, Minit, myInvenio, QPR ProcessAnalyzer, Lexmark’s Perceptive Process Mining and many more are now available. The availability and application of these tools illustrate the uptake of process mining.

The course uses many examples using real-life event logs to illustrate the concepts and algorithms. After taking this course, one is able to run process mining projects and have a good understanding of the Business Process Intelligence (BPI) field. Moreover, students will be able to directly apply process mining techniques in all kinds of practical settings, including internships and master projects.

### Objectives

After taking this course, students should:

- have a good understanding of Business Process Intelligence techniques (in particular process mining),
- understand the role of Big Data and Data Science in today’s society,
- be able to relate process mining techniques to other analysis techniques such as simulation, business intelligence, data mining, machine learning, and verification,
- understand the relation between process mining and data mining techniques like classification, clustering, and association rules,
- be able to apply basic process discovery techniques such as the alpha algorithm to learn a process model from an event log (both manually and using tools),
- understand how more advanced process discovery techniques like region-based mining, genetic mining, and heuristic mining work
- be able to apply basic conformance checking techniques (such as token-based replay) to compare event logs and process models (both manually and using tools),
- be able to extend a process model with information extracted from the event log (e.g., show bottlenecks),
- have a good understanding of the data needed to start a process mining project,
- be able to characterize the questions that can be answered based on such event data,
- explain how process mining can also be used for operational support (prediction and recommendation), and
- be able to execute process mining projects in a structured manner using the L* life-cycle model.

### Contact

[bpi@pads.rwth-aachen.de](mailto:bpi@pads.rwth-aachen.de?subject=bpi)