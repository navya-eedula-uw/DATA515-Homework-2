# Wikipedia Politician Articles Analysis

## Overview
The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. This assignment will consider articles on political figures from different countries. For this assignment, you will combine a dataset of Wikipedia articles with a dataset of country populations, and use a machine learning service called ORES to estimate the quality of each article.

You are expected to perform an analysis of how the coverage of politicians on Wikipedia and the quality of articles about politicians varies among countries. Your analysis will consist of a series of tables that show:

1.  The countries with the greatest and least coverage of politicians on Wikipedia compared to their population.

2.  The countries with the highest and lowest proportion of high quality articles about politicians.

3.  A ranking of geographic regions by articles-per-person and proportion of high quality articles.


You are also expected to write a short reflection on the project that focuses on how both your findings from this analysis and the process you went through to reach those findings helps you understand the causes and consequences of biased data in large, complex data science projects.

# Data Sources
1. The Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries. This data is in the homework folder as [politicians_by_country.AUG.2024.csv](https://drive.google.com/file/d/1UZ9QUYQ1R2T3Nzau85ToSNkvEzXwLUtf/view?usp=sharing).

2. The population data is available in CSV format as [population_by_country_AUG.2024.csv](https://drive.google.com/file/d/1PlBRdx1t2eSCymXmOqtWXFJPiMn_gTQS/view?usp=sharing) from the homework folder. This dataset was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau.

The [population_by_country_AUG.2024.csv](https://github.com/navya-eedula-uw/DATA515-Homework-2/blob/main/politicians_by_country_AUG.2024.csv) contains rows that provide cumulative regional population counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA). These rows should not match the country values in [politicians_by_country.AUG.2024.csv](https://github.com/navya-eedula-uw/DATA515-Homework-2/blob/main/population_by_country_AUG.2024.csv), but you will want to retain them so that you can report coverage and quality by region as specified in the analysis section below.

# API Documentation
### MediaWiki PageInfo API

The **MediaWiki PageInfo API** is used to retrieve detailed information about a Wikipedia article, such as the page ID, latest revision ID, and edit history. This API is essential for accessing metadata on any Wikipedia article based on its title. It supports various query parameters, allowing users to request specific details about pages for use in research or automated tasks. More information can be found in the MediaWiki Action API: PageInfo and the official MediaWiki API documentation.

### ORES API

The **ORES API** provides machine learning-driven assessments of Wikipedia article quality, using revision IDs to predict article completeness and reliability. It offers probability-based scores for various quality levels (e.g., Stub, Start, C-Class), aiding in the automated evaluation of article content. This is especially useful for tasks such as monitoring article quality over time or detecting potential areas for improvement. Further details are available in the ORES API Documentation and the [ORES API Reference](https://ores.wikimedia.org/v3/).

## Getting Access Token

### Get Access Token

To access **Lift Wing** (the ML API service), you will need a Wikimedia account. If you already have a Wikipedia account, you can use it, but if not, you’ll need to create one to obtain an access token. Follow this [official guide](https://www.mediawiki.org/wiki/Manual:Create_API_key) to create an API key for your account. Ensure you create a Personal API token instead of a Server-side app key. Once the access token is successfully created, you will receive a Client ID, Client Secret, and Access Token—save all three. If you lose them, you will have to deactivate the token and generate a new one.

In the data acquisition code, make sure to use your `username` and the `Access Token` you generated.

# License
Some portions of the code below are derived from examples created by Dr. David W. McDonald for the DATA 512 course in the UW MS Data Science degree program. This code is shared under the Creative Commons CC-BY license. Revision 1.2 - September 16, 2024.

A copy of the reference codes can be found in this repository within the folder labeled `code_references`.

This code is provided "as is," without any warranty of any kind. The author is not responsible for any issues that may arise from its use. Contributions to this project are welcome. Please submit a pull request or open an issue to discuss potential improvements and additions.

## Analysis Goals
The analysis will consist of calculating total-articles-per-capita (a ratio representing the number of articles per person) and high-quality-articles-per-capita (a ratio representing the number of high quality articles per person) on a country-by-country and regional basis.

In this analysis a country can only exist in one region. The `population_by_country_AUG.2024.csv` actually represents regions in a hierarchical order. For your analysis always put a country in the closest (lowest in the hierarchy) region.

For this analysis you should consider "high quality" articles to be articles that ORES predicted would be in either the "FA" (featured article) or "GA" (good article) classes.

Also, keep in mind that the `population_by_country_AUG.2024.csv` file provides population in millions. The calculated proportions in this step are likely to be very small numbers. Think carefully about how to represent the results.

### Questions to be Answered
1.  Top 10 countries by coverage: The 10 countries with the highest total articles per capita (in descending order) .

2.  Bottom 10 countries by coverage: The 10 countries with the lowest total articles per capita (in ascending order) .

3.  Top 10 countries by high quality: The 10 countries with the highest high quality articles per capita (in descending order) .

4.  Bottom 10 countries by high quality: The 10 countries with the lowest high quality articles per capita (in ascending order).

5.  Geographic regions by total coverage: A rank ordered list of geographic regions (in descending order) by total articles per capita.

6.  Geographic regions by high quality coverage: Rank ordered list of geographic regions (in descending order) by high quality articles per capita.

# Summary of Results
The analysis of Wikipedia coverage per capita across countries reveals significant disparities. For questions 1 and 2, the top 10 countries by article coverage show smaller nations, like **Antigua and Barbuda** with 330 articles per million people, leading the pack, indicating a high ratio of Wikipedia articles to population size. On the other hand, larger countries like **China**, with only 0.011 articles per million people, rank among the bottom 10, illustrating a vast gap in coverage. This suggests that countries with smaller populations or more active contributors tend to have higher article counts per capita, while barriers such as language, internet access, and emphasis on Wikipedia contributions may hinder coverage in larger nations.

In questions 3 and 4, focusing on high-quality articles (FA or GA status), we observe a similar trend. **Montenegro** leads with 5 high-quality articles per million people, indicating robust content curation and editing efforts. **Slovenia** rounds out the top 10 with 0.95 high-quality articles per million people. At the bottom of the list, **Bangladesh** has only 0.0058 high-quality articles per million people, reflecting a lack of detailed, well-researched content. The availability of high-quality articles often correlates with active, engaged contributor communities or editorial focus, while countries at the bottom may face challenges like fewer contributors or resource limitations for in-depth curation.

When shifting focus to geographic regions in questions 5 and 6, the **Caribbean** emerges as the region with the highest total Wikipedia article coverage, boasting 2,190 articles per million people. In contrast, **East Asia** shows minimal representation with only 0.108 articles per million people. The same pattern holds for high-quality articles, where the Caribbean again leads with 90 high-quality articles per million people, while East Asia lags with just 0.0021 high-quality articles per million people. Regions like the Caribbean may benefit from smaller populations and active local communities, whereas regions like East Asia may face challenges with fewer contributors to English Wikipedia, despite their larger populations.

# Usage Instructions
1. Clone the repository:
   ```
   git clone https://github.com/your-repo/data-512-homework_2.git
   cd data-512-homework_2
   ```
2. Install the external required Python libraries in python notebook cells using
`pip install <library_name>`

4. Place the `politicians_by_country_AUG.2024.csv` and `population_by_country_AUG.2024.csv` files in the project directory.

5. Run the Jupyter notebook:

    `jupyter notebook wikipedia_politicians_analysis.ipynb`

Ensure that you have the necessary API access for ORES by storing your API key securely in the environment or configuration file.

The notebook requires no additional configurations, allowing you to simply use the "Run All Cells" option in the Python Notebook.

# Research Implications

Initially, I expected countries with larger populations, like **China** and **India**, to have more articles due to a higher number of potential contributors. I also assumed that developed countries would have a greater share of high-quality articles, given their resources. However, it was surprising to see smaller countries like **Antigua and Barbuda** and **Micronesia** rank much higher in articles per capita than larger countries. This shows that even a modest number of articles can significantly impact per capita coverage in smaller nations.

Moreover, economically advanced countries like **Japan** and **Norway** had surprisingly low numbers of high-quality articles per capita, suggesting different cultural priorities in content curation. Additionally, the process of merging datasets revealed challenges, such as mismatches in country names between the articles and population data.

The analysis indicates that smaller nations, particularly in the **Caribbean** and **Oceania**, dominate the top rankings for articles per capita, while larger countries have lower coverage due to their vast populations. High-quality articles are more prevalent in smaller countries with active communities rather than in the most populous or developed regions.

These findings suggest that Wikipedia's content is driven more by community engagement than by country size or wealth. High-quality content depends on dedicated contributors, while larger countries may face language barriers or rely on local platforms. This highlights the role of contributor behavior and regional focus in shaping data coverage and biases.

Question: **What (potential) sources of bias did you discover in the course of your data processing and analysis?**

Potential sources of bias include uneven digital literacy and internet access across regions, which can lead to overrepresentation of countries with better connectivity while marginalizing those with limited digital infrastructure. Additionally, the political and cultural context affects contributions; regions with restrictions on political speech may have fewer articles, skewing the global narrative towards countries with more open environments. Language proficiency plays a crucial role, as English Wikipedia inherently favors countries with higher English proficiency, resulting in lower representation for non-English-speaking countries. Furthermore, some regions may have cultural norms that prioritize privacy or downplay documentation, leading to fewer articles about political figures.

Question: **What might your results suggest about the internet and global society in general?**
The analysis suggests that internet access, digital literacy, and language skills significantly influence global knowledge creation and dissemination. Countries with robust digital infrastructure are more likely to contribute to platforms like Wikipedia, while those with limited access and lower English proficiency are underrepresented. This reveals that, although the internet offers opportunities for open information sharing, it often mirrors existing global inequalities, amplifying the voices of regions with better access and highlighting the need for more equitable access to information and knowledge creation within global society.

Question: **How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?**
A researcher could address these biases by incorporating additional data on internet access and digital literacy rates to better understand regional disparities in Wikipedia contributions. Including information on English proficiency and analyzing data from non-English Wikipedia versions would provide a more comprehensive view of global coverage, mitigating biases toward English-speaking countries. Additionally, integrating socio-political context metrics, such as press freedom and political openness, could explain variations in article coverage and offer a deeper understanding of regional constraints. Lastly, cross-wiki analysis, including translations of articles from other language Wikipedias, could enrich the dataset with diverse perspectives, leading to a more balanced representation of political figures globally.

### File Structure

code_references/
├── wp_ores_liftwing_example.ipynb
├── wp_page_info_example.ipynb

generated_files/
├── articles_ores_scores.csv
├── articles_page_info.json

generated_output/
├── wp_countries-no_match.txt
├── wp_politicians_by_country.csv

ReadME.md
LICENSE
politicians_by_country_AUG.2024.csv
population_by_country_AUG.2024.csv
wikipedia_politicians_analysis.ipynb
