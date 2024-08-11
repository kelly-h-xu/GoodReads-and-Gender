# GoodReads-and-Gender
## An NLP Analysis of Gender Perceptions in Book Reviews, 2006-2017

## Project Background
💻 This project was created with Sarah Konrad, Sarah Ouda, Unzila Sakina, and Joyce Thomas. 

### Research Landscape + Research Questions
Despite pushes to diversify literature and calls for better representation of female characters, many prominent authors still face scorn for depicting characters as gender stereotypes with little depth. We wanted to see if gender bias was prevalent in the language GoodReads reviewers used, studying what common stereotypes were invoked and how strong the linguistic associations between gender and stereotypes were in reviews and how these varied by genre. We believe these findings could contribute to ongoing discourse surrounding publishing and gender stereotypes while also offering greater context to the biases present in reviews we rely on for book selection, which may not be immediately apparent.

For this project, our main research questions were:
1. What words/qualities that are common in literary stereotypes and character tropes do GoodReads reviewers attribute or associate with male and female characters across genres? 
2. What differences exist in the association of literary stereotypes with certain genders when we cross compare stereotypical associations with opposite genders across genres? 
3. How do ratings differ for female-centered vs. male-centered reviews per genre?

### Data 
For our project, we are using data from the “Goodreads Book Graph Datasets” (https://mengtingwan.github.io/data/goodreads.html) collection. The collection includes ~15 million reviews from 2006-2017, scraped from the GoodReads website and made publicly available by Mengting Wan, PhD, a Senior Research Scientist at Microsoft. These reviews were those publicly viewable from GoodReads users and all IDs were anonymized. These JSON files have breakdowns on authorIDs, timestamps of reviews, sentence content of reviews, userIDs, bookIDs, and the overall 5-star rating an individual gave of the book. All 15 million reviews are in the file named goodreads_reviews_dedup.json.gz, and these reviews are also split into smaller datasets by book genre (children's literature, poetry, romance, comics, or mystery/thriller/crime). We segmented our data by genre, both to save computational power (the romance dataset alone had ~3.5 million entries), as well as for the purposes of our analysis. We filtered our data with the langdetect library to filter out non-English reviews.

## Methods 
