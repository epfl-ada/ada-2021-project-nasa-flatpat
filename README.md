## FEMEDIA - Analyzing female representation in English-speaking written media

### Data Story:
website: https://vfayt99.github.io/femedia/

github repo: https://github.com/VFayt99/femedia

### Abstract
We believe that media outlets can only be unbiased, if the people in charge as well as the voices portrayed come from a diverse and society-representative background. Here we focus on gender representation in US media that is consumed with the higher frequency, defined based on online page views (https://mediabiasfactcheck.com/methodology/), with the help of Quotebank. Our analysis envisions a fine-grained approach to the overarching question of equal gender representation, which can be answered with an unhelpful NO.

Using the information about the quotes – the person being cited as well as the provenance of the news article – we explore the representation of female voices on three different levels: At the central level we investigate the ratio of quotes in the quotebank stemming from women under a variety of different conditions, such as political affiliation of the newspapers, readership size, etc. 
On a meta-level we also investigate the proportion of  female editors in the news outlets present, again in a detailed fashion across the accessible dimensions. These insights we link to the proportion of women cited within the respective newspapers.
Thirdly, utilizing the content of the quotes themselves we determine whether a difference in topics for which people of different gender are cited can be observed. Furthermore, with a similar method we investigate the different contextualization of women and men within the news stories and deduce potential stereotype propagations.
 
  
### Code Description
In the following, we describe the main functionality of the files in the repository.

```
.
├── README.md
├── main.ipynb: Main document for the exploratory and descriptive analysis
│   ├── Exploratory Statistics
|   ├── Database Enrichment with Wikidata and Bias Information of Newspapers
|   ├── Topic Analysis pipeline applied to Quotes issued by Male vs. Female speakers
├── notebooks:  <-- additional scripts for cleaning & running topic analysis on randomly sampled subset of data (1.5 Mio quotes)
|   ├── cleaning - initial data and cleaning (as in MS2)
|   ├── GDSMM-Final_Pipeline_and_topic_by_gender - Input: subsampled dataset, Output: NLP Preprocessed Dataset (Tokenization, Lemmatization, Generic stopword Removal) &   
|   |   Subsequent GDSMM Topic analysis for quotes issued by men and women
|   ├── GDSMM_Topics_about_Gender_plus_Statistics - Employ same pipeline as above except NLP preprocessing: Input: Preprocessed subsample, Output: GDSMM Topic analysis for 
|       quotes about men and women, Descriptive Statistics on Topics by different Genders and about different genders according to political bias and editor gender.
.
```
 
### Research Questions

#### Quote Issuers Analysis
- Proportion of females quoted in entirety of quotebank
    - Fine grained analysis across different topics found in quotes
        - Politics, global issues, economy, health, lifestyle, ...
        - Political affiliation - based on (https://mediabiasfactcheck.com/)

 - Language women use in quotes
     - Topic analysis: What do women speak about? And how is this with respect to men?
     - Does it differ in media of varying political inclination?
     - Or does it make a different for journals in which the person at the head of the editorial board is female or male?
     
#### Meta Analysis of Media
- Analysis of the proportion of female chief-editors in media mentioned quotebank weighted according to 
    - Overall
    - Political Spectrum

#### Analysis of quotes specifically mentioning men/women
- Determination of most common topics used in quotes where women versus men are mentioned: 
  Can a difference be observed and do these indicate portrayal of genders into certain stereotypes? 
      
#### Abandoned ideas
- We have considered also investigating the timeline of the effects in question, however, we dropped the idea, as 5 years are too short to expect big societal shifts. We are aware that MeToo started in October 2017, and this would be a potential temporal splitting point for the data we are interested in. Nevertheless, since we are more focusing on the representation of women in the media landscape and how media portray women, the debate around sexual harassment definitely will influence the way women are talked about (ie. women being more often referred to as victims due to the abundance of similar stories), but beyond this analysis, which we implicitly plan to capture, MeToo does not directly relate to our research question.
- We additionally investigated the top 10 nouns, verbs, adjectives used by women and men quotes as well as in quotes talking about the respective genders, but did not obtain very telling results. This may be due to the fact that the difference lies more in the distinct combination of the same words (see our topic analysis) than in the different words themselves. As we saw in the topic analysis, the "difference" in topics lay mostly in less prevalent ones - where the corresponding words would not make it into the top 10. The only excpetion was men top words having a few sports words included, but other than that it was mainly the ordering that was slightly different - nothing one could base any type of analysis.

### Additional datasets
- **Wikidata links**
We use the QID of the speaker to obtain information about the gender and further scrape Wikidata information about the newspapers (from their url) to get insights on readership and editors. We access the data using the SPARQL query service, implemented in Python. 

- **Bias ranking of English-speaking media**
In order to analyse the effect the bias of a newspaper has on female representation, we use a dataset (https://gist.github.com/nsfyn55/605783ac8de36f361fb10ef187272113)  extracted from https://mediabiasfactcheck.com, containing the bias ranking of most English-speaking media. The Methodology used by Media Bias / Fact Check is to both classify the political bias (from extreme left to extreme right – based on the US political landscape) as well as the Factual Reporting Score from Very High to Very low ( in 6 steps). 
From the github repository linked above, we obtained the dataset containing the site name, url, bias rating, and factual reporting and traffic score. 
We used this dataset this to filter and compare female representation across the scales made available via this classification.

- **Hand-annotated Editor dataset**
In addition to the metrics about the newspapers themselves, we also researched the editors-in-chief of the 147 HIGH-TRAFFIC media based on the Media Bias Fact Check classification to investigate the influence of the content decision maker's gender on the female representation.

    
### Methods
- VAEX – vaex.io/docs – fast on the fly data science tool to join all data, merge with other dataframes, perform filtering of the specific sub-analyses and save in an efficient manner for later continuation.
- Beautiful soup was used to scrape mediabiasfactcheck.com and generate the journal dataset 
- GSDMM ( Gibbs sampling algorithm for a Dirichlet Mixture Model)   -  Since we cannot use the much more known LDA package to extract topics from the quotes as neither the generally accepted threshold for LDA text size > 250 words nor the assumption that one text treats multiple topics hold for the quotes we wish to analyse, we have to resort to the lesser known GSDMM package (https://github.com/rwalk/gsdmm)
        - The disadvantage is that the algorithm is much less efficient than LDA making it less applicable to huge datasets due to long run times. 
        - If we encounter big runtime limitations we will resort to analyzing specific and representative subsets of our dataset.
            - Within the representation of women and men this should not be an issue as the number of quotes (see notebook) to treat is substantially reduced.
            - For establishing an overview of the topic dependence of female/male speakers we likely have to select the 10 biggest newspapers - from diverse political backgrounds (see our other database) as a proxy for the entire dataset.
- NLPTK, Spacy and Gensim - in order to handle the quotation data we used these three libraries.
- Integrated Vaex functions for plotting, Seaborn and Matplotlib plotting tools
- Dynamic diagrams were generated with the Plotly library
- Jekyll with the Bootstrap theme clean-blog was used to set up the datastory webpage


### Distribution of the work load
*disclaimer*: most of our work on this project was done using Google Colab and Drive, which is why the
amount of commits in this repository is (weirdly) low - everyone put in the work, we just found it made
more sense to work on Colab considering the amount of data we had !

- Patricia: 
    - Topic analysis on Quotes about Women/Men
    - Descriptive analysis topics
    - Designing and writing datastory, page design
- Natasa:
    - Data loading and subsampling
    - Descriptive Statistics US high traffic dataset, subsampled set, topics
    - Final organization notebooks
- Vladimir:
    - Descriptive Statistics US high traffic dataset and subsampled set
    - Crawling the web for media data
    - Setting up of the Jekyll theme on github, graphical design 
- Salome: 
    - Data loading, processing subsampling
    - GSDMM pipeline and topics analysis on Quotes by Women/Men
    - Writing datastory, fine tuning website
- All:
    - Hand-annotate editors and editor's gender
    - Finetuning website
