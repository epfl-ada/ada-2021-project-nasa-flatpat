## FEMEDIA - Analyzing female representation in English-speaking written media
 
### Abstract
We believe that media outlets can only be unbiased, if the people in charge, the workers as well as the voices portrayed come from a diverse and society-representative background. In this work we focus on gender representation in English-speaking media with the help of Quotebank. Our analysis envisions a fine-grained approach to the overarching question of equal gender representation, which can be answered with an unhelpful NO.
Using the information about the quotes – the person being cited as well as the provenance of the news article – we explore the representation of female voices on three different levels: At the central level we investigate the ratio of quotes in the quotebank stemming from women under a variety of different conditions (extracted from the newspaper additional information). For that we enriched Quotebank with Wikidata entries about the speakers as well as bias rankings/political spectrum information about most newspapers.
On a meta-level we also investigate the proportion of  female editors in the news outlets present, again in a detailed fashion across the accessible dimensions. These insights we link to the proportion of women cited within the respective newspapers.
Thirdly, utilizing the content of the quotes themselves we determine whether a difference in topics can be observed for which the different genders are cited. Furthermore, with a similar method we investigate the different contextualization of women and men within the news stories and deduce potential stereotype propagations.
 
  
### Code Description
In the following, we describe the main functionality of the files in the repository.

```
.
├── README.md
├── main.ipynb   
│   ├── Exploratory Statistics
|   ├── Database Enrichment with Wikidata and Bias Information of Newspapers
|   ├── Topic Analysis pipeline applied to Quotes issued by Male vs. Female speakers
├── 
├── experiments.ipynb

└── run.py
```
 
### Research Questions

## Quote Issuers Analysis
- Proportion of females quoted in entirety of quotebank
  ![gender_2020_speakers](https://user-images.githubusercontent.com/91182598/141517273-7de6f3fc-5df5-45f2-b72c-8872cdee49c5.png)

    - Fine grained analysis across different topics of quotes
        - International affairs, politics, tech, finance, “neighbourhood stories”, science, health,...
        - Types of journals
        - Size (global, national, local distribution of medium)
        - Political affiliation - based on (https://mediabiasfactcheck.com/)
 
        ![politics_ratio](https://user-images.githubusercontent.com/91182598/141517591-26231542-692c-4bcc-bf13-9f9d3f349d91.png)

 - Language women use in quotes
     - Topic analysis: What do women speak about? And how is this with respect to men?
     - Sentiment analysis: How do women speak about things? Positively, negatively,...
     - 
## Meta Analysis of Media
- Analysis of the proportion of female chief-editors in media mentioned quotebank weighted according to 
    - Readership Size
    - Political Spectrum
    - Theme (across selected news papers)
    - total
      ![gender_2020_editors](https://user-images.githubusercontent.com/91182598/141517470-e64c68fc-c549-431d-8eb0-8dd73e4fb690.png)

## Analysis of quotes specifically mentioning men/women
- Determination of most common words/topics used in quotes where women versus men are mentioned: 
  Can a difference be observed and do these indicate portrayal of genders into certain stereotypes? 
  
  One topic cluster for women-filtered quotes from the dataset of 2020 - women as assault victims
  ![test_word_maps_female_topic_0](https://user-images.githubusercontent.com/91182598/141510847-d37e6722-b1eb-45c1-a9d2-d1b723b428e6.png)
  
  An equally sourced word cluster for men - sports and youth
  ![test_word_maps_male_topic_1](https://user-images.githubusercontent.com/91182598/141510850-c1a44e76-f78a-4ca1-b394-764981af9740.png)

    
## Abandoned ideas
- We have considered also investigating the timeline of the effects in question, however, we dropped the idea, as 5 years are too short to expect big societal shifts. We are aware that MeToo started in October 2017, and this would be a potential temporal splitting point for the data we are interested in. Nevertheless, since we are more focusing on the representation of women in the media landscape and how media portray women, the debate around sexual harassment definitely will influence the way women are talked about (ie. women being more often referred to as victims due to the abundance of similar stories), but beyond this analysis, which we implicitly plan to capture, MeToo does not directly relate to our research question.


### Proposed additional datasets
- **Wikidata links**
We use the QID of the speaker to obtain information about the gender and further scrape Wikidata information about the newspapers (from their url) to get insights on readership and editors. We access the data using the SPARQL query service, implemented in Python. 

- **Bias ranking of English-speaking media**
In order to analyse the effect the bias of a newspaper has on female representation, we use a dataset (https://gist.github.com/nsfyn55/605783ac8de36f361fb10ef187272113)  extracted from https://mediabiasfactcheck.com, containing the bias ranking of most English-speaking media. The Methodology used by Media Bias / Fact Check is to both classify the political bias (from extreme left to extreme right – based on the US political landscape) as well as the Factual Reporting Score from Very High to Very low ( in 6 steps). 
From the github repository linked above, we obtained the dataset containing the site name, url, bias rating, and factual reporting score, to which we added the popularity level (minimal, medium, or high traffic). This dataset still needs to be curated, as new newspapers have been evaluated since the dataset was created. We plan to do this either manually or by doing further web scraping, depending on which journals we choose to filter on and how much data we are missing.
 We will use this to filter and compare female representation across the scales made available via this classification.

    
    
### Methods
- VAEX – vaex.io/docs – fast on the fly data science tool to join all data, merge with other dataframes, perform filtering of the specific sub-analyses and save in an efficient manner for later continuation.
- nltk.VADER – Sentiment analysis package that is part of one of the most common natural language processing toolkits in python.
The package is widely used and can handle big amounts of data. Further it is pretrained on Social Media texts such as Twitter posts (short texts in the same size range as quotes) and can be easily adapted/extended to slightly different types of texts. 
- GSDMM ( Gibbs sampling algorithm for a Dirichlet Mixture Model)   -  Since we cannot use the much more known LDA package to extract topics from the quotes as neither the generally accepted threshold for LDA text size > 250 words nor the assumption that one text treats multiple topics hold for the quotes we wish to analyse, we have to resort to the lesser known GSDMM package (https://github.com/rwalk/gsdmm)
        - The disadvantage is that the algorithm is much less efficient than LDA making it less applicable to huge datasets due to long run times. 
        - If we encounter big runtime limitations we will resort to analyzing specific and representative subsets of our dataset.
            - Within the representation of women and men this should not be an issue as the number of quotes (see notebook) to treat is substantially reduced.
            - For establishing an overview of the topic dependence of female/male speakers we likely have to select the 10 biggest newspapers - from diverse political backgrounds (see our other database) as a proxy for the entire dataset.
- NLPTK, Spacy and Gensim - in order to handle the quotation data we used these three libraries.
- Integrated Vaex functions for plotting, Seaborn and Matplotlib plotting tools

 
### Proposed timeline
- by Nov. 19th:
    - Extended descriptive statistics to entire dataset
        - male-female ratio of chief-editors
        - male-female quotes ratio in:
        - different types of newspapers
        - readership
        - political spectrum
        - degree of bias (based on our database)
        - according to editor’s gender of newspaper
    - Global Topic analysis across all male and female speakers + comparison
    - Sentiment analysis across all male and female speakers  + comparison
- by Nov 23rd:  
    - focus on HW2
- by Dec 3rd: 
    - Topic analysis of quotes referring to women or men (ie. content of the quote, not the speaker)
        - differences between selected newspapers (to be determined by which newspapers are emblematic (see analyses of week before)
    - Initializing github pages and textual description of obtained results
- by Dec 10th:     
    - Optimizing plots to optimize understanding of key results
    - Textual description
    - Layout of GitHub
- by Dec 17th: 
    - finalizing description and website and hand-in


### Organization within the team
- Salome: 
    - GSDMM Analysis of Topics
    - jointly with Patricia GSDMM Analysis of Quotes specifically talking about women/men
- Patricia: 
    - Representation of women within text - Topic analysis
    - Initialization of github pages
    - together with Natasa: Descriptive Statistics
- Natasa:
    - Descriptive Statistics finalization
    - Sentiment Analysis
- Vladimir:
    - Support in Topic Analysis
    - Fine-tuning of plotting
-  all:
    - Description /Textual presentation
