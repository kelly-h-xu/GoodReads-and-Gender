# GoodReads-and-Gender
An NLP Analysis of Gender Perceptions in Book Reviews, 2006-2017

## Project Overview
👏 This project was created with Sarah Konrad, Sarah Ouda, Unzila Sakina, and Joyce Thomas.  

💻 Text Vectorization + Deep Learning using Word2Vec, NLTK Toolkit, and Sci-Kit Learn. Data wrangling and cleaning using Pandas. Data visualization using Seaborn.   

➡️ Limitations/Disclaimer: 

### Research Landscape + Research Questions
Despite pushes to diversify literature and calls for better representation of female characters, many prominent authors still face scorn for depicting characters as gender stereotypes with little depth. We wanted to see if gender bias was prevalent in the language GoodReads reviewers used, studying what common stereotypes were invoked and how strong the linguistic associations between gender and stereotypes were in reviews and how these varied by genre. We believe these findings could contribute to ongoing discourse surrounding publishing and gender stereotypes while also offering greater context to the biases present in reviews we rely on for book selection, which may not be immediately apparent.

For this project, our main research questions were:
1. What words/qualities that are common in literary stereotypes and character tropes do GoodReads reviewers attribute or associate with male and female characters across genres? 
2. What differences exist in the association of literary stereotypes with certain genders when we cross compare stereotypical associations with opposite genders across genres? 
3. How do ratings differ for female-centered vs. male-centered reviews per genre?

### Data 
For our project, we are using data from the “Goodreads Book Graph Datasets” (https://mengtingwan.github.io/data/goodreads.html) collection. The collection includes ~15 million reviews from 2006-2017, scraped from the GoodReads website and made publicly available by Mengting Wan, PhD, a Senior Research Scientist at Microsoft. These reviews were those publicly viewable from GoodReads users and all IDs were anonymized. These JSON files have breakdowns on authorIDs, timestamps of reviews, sentence content of reviews, userIDs, bookIDs, and the overall 5-star rating an individual gave of the book. All 15 million reviews are in the file named goodreads_reviews_dedup.json.gz, and these reviews are also split into smaller datasets by book genre (children's literature, poetry, romance, comics, or mystery/thriller/crime). We segmented our data by genre, both to save computational power (the romance dataset alone had ~3.5 million entries), as well as for the purposes of our analysis. We filtered our data with the langdetect library to filter out non-English reviews.

## Methods 
### Cleaning
	We began by converting all of the original JSON data files into Pandas dataframes, before filtering, segmenting, and exporting reviews into separate CSV by the genres that they were tagged with. The five genres we used were: children's, romance, mystery, comics, and poetry. We converted the CSVs back into Pandas dataframes and then filtered out the non-English reviews using the langdetect library. Then, to prepare the GoodReads reviews for vectorization (Word2Vec), we lowercased all words in the reviews, stripped them of all punctuation and numbers, removed random symbols, removed all stopwords as recommended by the NLTK documentation (words that are incredibly common and obfuscate associations ex. "to", "for", "the"), and lemmatized our texts with the NLTK lemmatizer so that all plurals were made singular and all verb tenses changed to simple present. 
### Building Word2Vec Models (led by Sarah Konrad)
 With the texts of our GoodReads reviews sufficiently cleaned, wrangled, and prepared, we built 5 total Word2Vec models, crafting an individual neural network of linguistic associations for each genre. Given the disparity in sizes of our datasets, we trained all of our Word2Vec models with a different number of epochs so that they hit a minimum of the 9 million text threshold set by GloVe's guidance for our linguistic associations by genre to ensure that we had stable and accurate word vectors. 
   With training complete, we then selected words associated with literary stereotypes of male and female characters to track in our analysis. Drawing on scholarship from literary scholar D. Jill Savitt in this article about female stereotypes in literature we selected the following twenty-two words to test associations: ["virgin", "old", "whore", "maid", "caring", "loving", "witch", "naive", "ideal", "sexy", "damsel", "innocent", "seductive", "nurturing", "pure", "temptress", "submissive", "obedient", "beautiful","ugly","docile"]. Building upon this thesis on stereotypes of male protagonists by Aimee Meuchel we selected the following twenty-two words to analyze archetypes of men: ["strong", "warrior", "soldier", "dominant", "sexy", "heroic", "hero", "power", "intelligent", "smart", "rich", ["logical", "rational", "fearless", "assertive", "brave", "protector", "tough",  "muscular", "honorable", "charismatic", "daring"]. We also selected collections of words to encapsulate male/female identity for our analysis by picking words related to gender or gendered concepts. Our collection for women was: ["she", "her", "herself", "girl", "woman", "hers", "femininity", "feminine","female", "girlfriend", "mother"] and our collection for men was: ["he", "him", "his", "himself", "boy", "man", "masculinity", "masculine", "male", "boyfriend", "father"]. These words were intended to be counterparts to each other. 
### Preliminary Data Analysis: Analyzing Cosine Similarity (led by Sarah Konrad)
	To discover differences in association, we then created heatmaps that calculated the cosine similarities between 11 of our gender stereotype words to 11 gender words at a time. Cosine similarity is a measure of the similarity of orientation of two vectors of words between [-1,1], meaning that the more similar the vector, the higher the cosine similarity, and therefore the closer in meaning/the more often two words are used in similar contexts. Perfectly opposite vectors (words with opposite meanings/associations) have a cosine similarity of -1, vectors that are 90 degrees different in orientation have a cosine similarity of 0, and vectors that match exactly (words with the same meanings/associations) have a cosine similarity of 1. The darker the cell on one of our heatmaps (the number is also written on it), the higher the cosine similarity between two words. 
 We created heatmaps that looked like the following for each genre, comparing female stereotypes to our women words, male stereotypes to our men words, and then vice versa with the female stereotypes to our men words and male stereotypes to women words. This enabled us to see how strong anticipated gender correlations were for certain words versus opposite gender correlations. We saved the results of these heatmaps in dataframes.*See appendix for all visualizations. In this example we see that "whore", "virgin", "naive", "damsel", "maid", and "ideal" all have moderate-strong correlations with at least one female gender word, suggesting a linguistic association between the stereotype and words related to women in GoodReads romance reviews. We also see that the words "whore", "caring", and "sexy" also have moderate-strong correlations with at least one male gender word, suggesting that these words may not necessarily be female stereotypes alone. 
**Ex: Correlation of Female and Male Words with Female Stereotypes in Romance Reviews**
[insert image here]
### Further Data Analysis: Analyzing Differences in Cosine Similarity (led by Kelly Xu)
  We recognized that we could not draw conclusions from a certain cosine similarity value (between a gendered word and its gender stereotype) alone.  For example, a gendered word might have high cosine similarity to a stereotype, but if both genders are equally correlated to that gender stereotype, it would mean readers associate both genders with it equally, and therefore it could not be deemed a specific stereotype in that genre. Using the gender-stereotype dataframes/heatmaps constructed in the previous part, we computed the average difference in cosine similarity between gender and stereotype. For example, we calculated the cosine similarity of women-words to female stereotypes minus cosine similarity of men-words to female stereotypes. Since our dataframes holding cosine similarity values have either columns denoting women words (“she”, “her”, “herself”…) or columns denoting men words (“he”, “him”, “himself”...), we paired linguistic counterparts together (ie. “she” and “he”, “her” and “him”) and took pairwise differences for each associated stereotype. With these pairwise differences subtracted for both male and female stereotypes, we built heatmaps showing the differences in association by gender. Here is an example for the romance genre, the left showing how much more women words are associated with female stereotypes, and the right showing how much more associated with male stereotypes male words are than female words. 

For example, in the female stereotype map, we see several swaths of dark purple, indicating that there are many instances in which female stereotypes are far more heavily correlated with female gender words compared to men (as much as 0.41 higher in one of the cells for ‘naive’). We also see a lot of dark purple on the male stereotypes correlation map, especially in the "dominant" column, suggesting that male gender words are far more correlated with that stereotype than female gender words, and thus it is a male stereotype. These are not our sole heatmaps, but they serve as an example of some of our preliminary results where clear swaths of gender stereotype correlation were present on our heatmaps. 
[insert image here]

To quantify the percentage degree of similarity, we then computed the mean difference in cosine similarity across all gendered words for each gender stereotype word, and plotted the results as bar plots. We decided to interpret negative cosine similarities as showing negative correlation (ie., women are less correlated with a certain anticipated stereotype than men are).  Positive correlations indicate a greater correlation of the gender anticipated to be associated with the stereotype with the stereotype. Below is an example of these visualizations for the romance genre.
[insert image here]


Visually analyzing genre-specific cosine similarity graphs, we found that association of gender with stereotypes varied somewhat by book genre. For our purposes, we considered a difference in cosine similarity greater than or equal to 0.05 to be significant evidence of correlation in a certain direction within the linguistic associations for a given genre, as it means that a minimum of a 5% greater cosine similarity was present, and such a percentage increase is not trivial given that thousands of words come between even a 0.01 cosine similarity increase given the size of vocabularies our Word2Vec models contained (each with a minimum of 80k words). This table shows the words that we observed a significant correlation in. 
[insert screenshot of the findings]

	Performing the analysis on the entire dataset of gendered words and stereotypes across genres, and now designating a mean cosine similarity greater than or equal to 0.4 as showing strong correlation in either direction, we found that women are most strongly correlated with stereotypes of: witch, damsel, maid, and beautiful. Men are most strongly correlated with stereotypes of: heroic, soldier, hero, charismatic, and honorable (see appendix for the exact correlation numbers). This indicates that there are significant linguistic associations between anticipated gender stereotypes in literature and words indicating gender in GoodsReads reviews across genres. 
	Correlation of female stereotypes and female words
	[insert image]
  Correlation of male stereotypes and male words
  [insert image]

### Analyzing Impact of Gender Stereotypes on Ratings (led by Sarah Ouda)
We wanted to get a sense of the distribution of ratings depending on whether or not a review was identified as male or female centered. In order to determine whether a review was male or female-centered, we developed a classification process utilizing the language of each review. We began by performing preliminary pre-processing on the text data, replacing the common contractions “she'll", "she'd", "he'll", and "he'd" with “she” and “he”. We then tokenized reviews by splitting them into a list of words and filtered for words that contain gendered language. Specifically, we searched for the presence of words such as "he", "him", "his", "mr", "her", "she", "hers", and "mrs". Within this filtered dataset, we standardized text by removing all forms of string punctuation and converting all words to lowercase. 
Next, we took the count of each gendered word within the tokenized review and developed a criteria based on weights to determine what constituted for a male/female-centered review. Gendered words that were unlikely to be caused by a typo, such as “him” and “his,” or “her” and “hers,” were assigned a higher weight of 2. Words that could easily be misspelled, such as “he” vs “her” or “mr” vs “mrs,” were assigned a lower weight of 1. We split the gendered words into male vs female lists (["he", "him", "his", "mr"] and ["her", "she", "hers", "mrs"]) and multiplied the occurrence of each word by its weight, resulting in two variables: a female and a male weight for each review. The final step was to simply classify the reviews based on which weight was higher. For example, if a review had a higher female weight, it would get classified as “0” (for female), otherwise as “1” (for male). For each genre we considered, we compared the distributions of reviews classified as male vs female (*See appendix for visual) and considered the difference in the ratings given by a reviewer depending on whether their review was male or female focused. After classifying the reviews of each of our considered genres, in looking at the distribution of ratings by gender, it does not appear that female/male centered reviews tend to be more highly rated than the other—the distribution of ratings look roughly the same regardless of gender for each genre (see appendix for distributions of all genres). 

Rating distributions for the Children Genre and the Romance Genre

PART V - Limitations and Future Work
	One limitation of our analyses was that we lacked the computational power to process all the book review data at once, taking up to 5 hours to clean the romance dataset alone. As a consequence, we were unable to truly generate generalizable linguistic findings across all GoodReads reviews and we were confined to working with genre subsets and centering our analysis and findings on those subsets. Future work should dedicate the computational power to clean and prepare 15 million reviews into one corpus to seek to better understand how language and gender stereotypes are present across GoodReads reviews as a whole, regardless of genre. 
	Another significant limitation of our linguistic analysis was that we primarily confined ourselves to pronouns like she, him, his, hers, etc. to measure gender, as such words are deployed to indicate the gender of a character/subject of a sentence in a review. However, there were a multitude of reviews in which these words did not appear, as review posters opted to call characters by their names instead. Although it is impossible to always tell the gender of a character given their name, building a dictionary of common and slightly less common male and female names to be replaced with gendered pronouns in GoodReads reviews would have strengthened our analysis and provided greater insight into linguistic associations between gender and stereotypes as it would have caught more instances where a character's gender is mentioned. Future work should make use of such a dictionary and seek to replace as many names as possible with equivalent gender pronouns to create even more accurate vectors in a Word2Vec model. 
	In the future, we would like to analyze whether gendered language in book description could help us predict reviewers’ association of gendered words with certain stereotypes. This question could be expanded on by joining genre datasets and the book description dataset (goodreads_book_works.json.gz) on the book_id field, however we did not make use of this dataset in our work. Additionally, we would like to investigate how genderqueer people are represented in literature in future work. While it is fairly easy to use gendered pronouns to analyze male and female stereotypes, we wish to acknowledge that not all characters or people fit into this gender binary and that linguistic associations for such individuals require further work to identify and model as gendered pronouns do not provide such a neat equivalency between their identity and stereotype. This work is also increasingly possible as there has been an ever-widening representation of gender-queer and non-binary people in the literary landscape in the past decade that shows no sign of slowing down. 
	Last, our most obvious limitation is that cosine similarity is not a perfect measure of linguistic similarity, as language does not always translate beautifully to math. While vectorization of words provides a stable algorithm to find connections between mathematical representations, linguistics have greater human nuance behind them that machines cannot always capture. Furthermore, cosine similarity only captures a measure of similarity for the orientation of vectors, and it does not capture magnitude (ie distance). Future work should also study the euclidean distance between word vectors to better understand how similar words are according to vectorization by looking at both the orientation and magnitude of vectors. 
PART VI- Conclusion
	We analyze book review data collected from 2006 to 2017 using Natural Language Processing techniques and find substantial evidence that book reviewers associate gendered words (viewing gender as a male or female binary for the purposes of this study) with certain gender stereotypes, and that these associations, along with the strength of association, vary by genre. We also examined the relationship between male or female centric reviews and their distribution of ratings, but found no significant correlation relating higher or lower ratings to a specific gender. 





Appendix- Visualizations by Genre, Bar Graphs of Difference in Similarity, and Ratings Graphs 

Romance
Female Words, Female Stereotypes




Male Words, Male Stereotypes






Cross Comparison: Female-Male Against Female Stereotypes 




Cross Comparison: Male-Female Against Female Stereotypes 





Bar Graphs of Mean Cosine Similarity to show Gender Comparisons - Female-Male on Female Stereotypes 





Bar Graphs of Cosine Similarity to show Gender Comparisons - Male-Female on Female Stereotypes 





Mystery
Female Words, Female Stereotypes




Male Words, Male Stereotypes






Cross Comparison: Female-Male Against Female Stereotypes 





Cross Comparison: Male-Female Against Male Stereotypes 





Bar Graphs of Mean Cosine Similarity to show Gender Comparisons - Female-Male on Female Stereotypes 





Bar Graphs of Cosine Similarity to show Gender Comparisons - Male-Female on Male Stereotypes 





Poetry
Female Words, Female Stereotypes




Male Words, Male Stereotypes






Cross Comparison: Female-Male Against Female Stereotypes 





Cross Comparison: Male-Female Against Male Stereotypes 





Bar Graphs of Mean Cosine Similarity to show Gender Comparisons - Female-Male on Female Stereotypes 





Bar Graphs of Cosine Similarity to show Gender Comparisons - Male-Female on Male Stereotypes 





Comics
Female Words, Female Stereotypes




Male Words, Male Stereotypes






Cross Comparison: Female-Male Against Female Stereotypes 





Cross Comparison: Male-Female Against Male Stereotypes 





Bar Graphs of Mean Cosine Similarity to show Gender Comparisons - Female-Male on Female Stereotypes 





Bar Graphs of Cosine Similarity to show Gender Comparisons - Male-Female on Male Stereotypes 





Children
Female Words, Female Stereotypes




Male Words, Male Stereotypes






Cross Comparison: Female-Male Against Female Stereotypes 





Cross Comparison: Male-Female Against Male Stereotypes 





Bar Graphs of Mean Cosine Similarity to show Gender Comparisons - Female-Male on Female Stereotypes 





Bar Graphs of Cosine Similarity to show Gender Comparisons - Male-Female on Male Stereotypes 





All Genres 
Bar Graphs of Cosine Similarity to show Gender Comparisons- Female-Male on Female Stereotypes










Bar Graphs of Cosine Similarity to show Gender Comparisons- Male-Female on Male Stereotypes









Bar Graphs of Distribution of Gender Classification of Reviews by Genre 

Bar Graphs of Distribution of Ratings by Gender Classification for each Genre 

