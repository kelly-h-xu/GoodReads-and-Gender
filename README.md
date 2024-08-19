# GoodReads-and-Gender
***An NLP Analysis of Gender Perceptions in Book Reviews, 2006-2017***

We analyze book review data collected from 2006 to 2017 using Natural Language Processing techniques and find substantial evidence that book reviewers associate gendered words (viewing gender as a male or female binary for the purposes of this study) with certain gender stereotypes, and that these associations, along with the strength of association, vary by genre. We also examine the relationship between male or female centric reviews and their distribution of ratings, but find no significant correlation relating higher or lower ratings to a specific gender. 

## Project Overview
👏 This project was created with Sarah Konrad, Sarah Ouda, Unzila Sakina, and Joyce Thomas.  

💻 Python with Word2Vec, NLTK Toolkit, and Sci-Kit Learn for Text Vectorization + Deep Learning. Pandas for data wrangling and data cleaning. Seaborn for data visualization.   

➡️ Limitations/Disclaimer: A big limitation in our linguistic analysis was viewing gender as a binary. Another significant limitation of our analysis was that we primarily confined ourselves to pronouns like she, him, his, hers, etc. to measure gender. However, there were many reviews in which these words did not appear, as review posters opted to call characters by their names instead.     

   Another limitation is that cosine similarity is not a perfect measure of linguistic similarity, as the algorithm cannot fully capture the nuance behind human language. Furthermore, cosine similarity only captures a measure of similarity for the orientation of vectors, and it does not capture magnitude (ie. distance). Future work should also study the euclidean distance between word vectors to better understand how similar words are according to vectorization by looking at both the orientation and magnitude of vectors. 

## Project Video
<a href="https://duke.box.com/s/vluzcj5pwqwcj332jd0l3wzox5mwa3oj" target="_blank">
    <img src="https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/goodreads_gender_video_thumbnail.png" alt="project video" width="400" height="250"/>
</a>

## Data 
For our project, we are using data from the “Goodreads Book Graph Datasets” (https://mengtingwan.github.io/data/goodreads.html) collection. The collection includes ~15 million reviews from 2006-2017, scraped from the GoodReads website and made publicly available by Mengting Wan, PhD, a Senior Research Scientist at Microsoft. These reviews were those publicly viewable from GoodReads users and all IDs were anonymized.   

To recreate our analysis, separately download the reviews datasets by genre (poetry, mystery, romance, comics, and children's), and ensure they are in the same directory as the code that cleans the data. 

## Research Question 1: What words/qualities that are common in literary stereotypes and character tropes do GoodReads reviewers attribute or associate with male and female characters across genres? 

### Cleaning
   We began by converting all of the original JSON data files into Pandas dataframes, before filtering, segmenting, and exporting reviews into separate CSV by the genres that they were tagged with. The five genres we used were: children's, romance, mystery, comics, and poetry. We converted the CSVs back into Pandas dataframes and then filtered out the non-English reviews using the langdetect library. Then, to prepare the GoodReads reviews for vectorization (Word2Vec), we lowercased all words in the reviews, stripped them of all punctuation and numbers, removed random symbols, removed all stopwords as recommended by the NLTK documentation (words that are incredibly common and obfuscate associations ex. "to", "for", "the"), and lemmatized our texts with the NLTK lemmatizer so that all plurals were made singular and all verb tenses changed to simple present. 
### Building Word2Vec Models 
   With the texts of our GoodReads reviews sufficiently cleaned, wrangled, and prepared, we built 5 total Word2Vec models, crafting an individual neural network of linguistic associations for each genre. Given the disparity in sizes of our datasets, we trained all of our Word2Vec models with a different number of epochs so that they hit a minimum of the 9 million text threshold set by GloVe's guidance for our linguistic associations by genre to ensure that we had stable and accurate word vectors. 
   With training complete, we then selected words associated with literary stereotypes of male and female characters to track in our analysis. Drawing on scholarship from literary scholar D. Jill Savitt in this article about female stereotypes in literature we selected the following twenty-two words to test associations: ```["virgin", "old", "whore", "maid", "caring", "loving", "witch", "naive", "ideal", "sexy", "damsel", "innocent", "seductive", "nurturing", "pure", "temptress", "submissive", "obedient", "beautiful","ugly","docile"]```. Building upon this thesis on stereotypes of male protagonists by Aimee Meuchel we selected the following twenty-two words to analyze archetypes of men: ```["strong", "warrior", "soldier", "dominant", "sexy", "heroic", "hero", "power", "intelligent", "smart", "rich", "logical", "rational", "fearless", "assertive", "brave", "protector", "tough",  "muscular", "honorable", "charismatic", "daring"]```. We also selected collections of words to encapsulate male/female identity for our analysis by picking words related to gender or gendered concepts. Our collection for women was: ```["she", "her", "herself", "girl", "woman", "hers", "femininity", "feminine","female", "girlfriend", "mother"]``` and our collection for men was: ```["he", "him", "his", "himself", "boy", "man", "masculinity", "masculine", "male", "boyfriend", "father"]```. These words were intended to be counterparts to each other. 
### Preliminary Data Analysis: Analyzing Cosine Similarity 
   To discover differences in association, we then created heatmaps that calculated the cosine similarities between 11 of our gender stereotype words to 11 gender words at a time. Cosine similarity is a measure of the similarity of orientation of two vectors of words between [-1,1], meaning that the more similar the vector, the higher the cosine similarity, and therefore the closer in meaning/the more often two words are used in similar contexts. Perfectly opposite vectors (words with opposite meanings/associations) have a cosine similarity of -1, vectors that are 90 degrees different in orientation have a cosine similarity of 0, and vectors that match exactly (words with the same meanings/associations) have a cosine similarity of 1. The darker the cell on one of our heatmaps (the number is also written on it), the higher the cosine similarity between two words. 
 We created heatmaps that looked like the following for each genre, comparing female stereotypes to our women words, male stereotypes to our men words, and then vice versa with the female stereotypes to our men words and male stereotypes to women words. This enabled us to see how strong anticipated gender correlations were for certain words versus opposite gender correlations.   
 **Comparing Female Stereotypes to Women Words (Romance Genre)**  
 ![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/romance_female_heatmap.png)  
 **Comparing Female Stereotypes to Men Words (Romance Genre)**  
 ![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/romance_male_heatmap.png)  
 In the example above, we see that "whore", "virgin", "naive", "damsel", "maid", and "ideal" all have moderate-strong correlations with at least one female gender word, suggesting a linguistic association between the stereotype and words related to women in GoodReads romance reviews. We also see that the words "whore", "caring", and "sexy" also have moderate-strong correlations with at least one male gender word, suggesting that these words may not necessarily be female stereotypes alone. 

### Further Data Analysis: Analyzing Differences in Cosine Similarity 
   We recognized that we could not draw conclusions from a certain cosine similarity value (between a gendered word and its gender stereotype) alone. If both genders are equally correlated to a certain gender stereotype, it would mean readers associate both genders with it equally, and therefore it could not be deemed a specific stereotype in that genre.    
   To verify true correlation with a stereotype, we took our heatmaps "Comparing Female Stereotypes to Women Words" and "Comparing Female Stereotypes to Men Words" and subtracted them. We also took our heatmaps "Comparing Male Stereotypes to Men Words" and "Comparing Male Stereotypes to Women Words" and subtracted them. Since the columns of our cosine similarity dataframes denote either women words (“she”, “her”, “herself”…) or men words (“he”, “him”, “himself”...), we paired linguistic counterparts together (ie. “she” and “he”, “her” and “him”) and took pairwise differences for each associated stereotype. Each value in the heatmap containing the results of this operation, can be understood as the average difference in cosine similarity between gender and stereotype. 
   
**How Much More Are Women Words Associated With Female Stereotypes, Than Men Words Are? (Romance Genre)**  
![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/romance_compare_female_to_male_heatmap.png)  
**How Much More Are Men Words Associated With Male Stereotypes, Than Women Words Are? (Romance Genre)**  
![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/romance_compare_male_to_female_heatmap.png)  
  

   For example, in the female stereotype map, we see several swaths of dark purple, indicating that there are many instances in which female stereotypes are far more heavily correlated with female gender words compared to men (as much as 0.41 higher in one of the cells for ‘naive’). We also see a lot of dark purple on the male stereotypes correlation map, especially in the "dominant" column, suggesting that male gender words are far more correlated with that stereotype than female gender words, and thus it is a male stereotype. These are not our sole heatmaps, but they serve as an example of some of our preliminary results where clear swaths of gender stereotype correlation were 
   
   We then computed the mean difference in cosine similarity across all gendered words for each gender stereotype word, and plotted the results as bar plots. We decided to interpret negative cosine similarities as showing negative correlation (ie., women are less correlated with a certain anticipated stereotype than men are).  Positive correlations indicate a greater correlation of the gender anticipated to be associated with the stereotype with the stereotype. Below is an example of these visualizations for the romance genre.  
     
**How Much More Are Women Associated With Female Stereotypes, Than Men Are? (Romance Genre)**
![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/romance_compare_female_to_male_bargraph.png)  

**How Much More Are Men Associated With Male Stereotypes, Than Women Are? (Romance Genre)**
![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/romance_compare_male_to_female_bargraph.png)  

## Results
### Cross-Genre Analysis
   Visually analyzing genre-specific cosine similarity graphs, we found that association of gender with stereotypes varied somewhat by book genre. For our purposes, we considered a difference in cosine similarity greater than or equal to 0.05 to be significant evidence of correlation in a certain direction within the linguistic associations for a given genre, as it means that a minimum of a 5% greater cosine similarity was present, and such a percentage increase is not trivial given that thousands of words come between even a 0.01 cosine similarity increase given the size of vocabularies our Word2Vec models contained (each with a minimum of 80k words). This table shows the words that we observed a significant correlation in. 
![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/cross_genre_analysis_1.png)
![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/cross_genre_analysis_2.png) 

### All Genres Analysis
   Performing the analysis on the entire dataset of gendered words and stereotypes across genres, and now designating a mean cosine similarity greater than or equal to 0.4 as showing strong correlation in either direction, we found that women are most strongly correlated with stereotypes of: witch, damsel, maid, and beautiful. Men are most strongly correlated with stereotypes of: heroic, soldier, hero, charismatic, and honorable (see appendix for the exact correlation numbers). This indicates that there are significant linguistic associations between anticipated gender stereotypes in literature and words indicating gender in GoodsReads reviews across genres. 
   **Overall Correlation of female stereotypes and female words**  
   ![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/overall_compare_female_to_male_bargraph.png)  
   **Overall Correlation of male stereotypes and male words**  
  ![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/overall_compare_female_to_male_bargraph.png)  

## Research Question 2: How do ratings differ for female-centered vs. male-centered reviews per genre? 
We wanted to get a sense of the distribution of ratings depending on whether or not a review was identified as male or female centered. In order to determine whether a review was male or female-centered, we developed a classification process utilizing the language of each review. We began by performing preliminary pre-processing on the text data, replacing the common contractions “she'll", "she'd", "he'll", and "he'd" with “she” and “he”. We then tokenized reviews by splitting them into a list of words and filtered for words that contain gendered language. Specifically, we searched for the presence of words such as "he", "him", "his", "mr", "her", "she", "hers", and "mrs". Within this filtered dataset, we standardized text by removing all forms of string punctuation and converting all words to lowercase.   

Next, we took the count of each gendered word within the tokenized review and developed a criteria based on weights to determine what constituted for a male/female-centered review. Gendered words that were unlikely to be caused by a typo, such as “him” and “his,” or “her” and “hers,” were assigned a higher weight of 2. Words that could easily be misspelled, such as “he” vs “her” or “mr” vs “mrs,” were assigned a lower weight of 1. We split the gendered words into male vs female lists (["he", "him", "his", "mr"] and ["her", "she", "hers", "mrs"]) and multiplied the occurrence of each word by its weight, resulting in two variables: a female and a male weight for each review. The final step was to simply classify the reviews based on which weight was higher. For example, if a review had a higher female weight, it would get classified as “0” (for female), otherwise as “1” (for male).   
For each genre we considered, we compared the distributions of reviews classified as male vs female and considered the difference in the ratings given by a reviewer depending on whether their review was male or female focused.     

### Results
After classifying the reviews of each of our considered genres, in looking at the distribution of ratings by gender, it does not appear that female/male centered reviews tend to be more highly rated than the other—the distribution of ratings look roughly the same regardless of gender for each genre.   

![](https://github.com/kelly-h-xu/GoodReads-and-Gender/blob/main/images/all_ratings_graphs.png)  







