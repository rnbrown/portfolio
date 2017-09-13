# Scraping Indeed.com and Predicting Salary

For this project, I built a web scraper to crawl through data science job postings in 16 major cities across the US. I parsed out the job title, company name, location, and salary range (if there was one posted) from the raw html data and stored the relevant information in a csv file. Salary ranges were posted in several different formats, so I had to use regex to parse those out.

After scraping the job data, I built a couple different models to predict if the job salary would be above or below the median salary based on the other fields I scraped. I used natural language processing to transform the job titles into data suitable for prediction. I then used my model to determine which job titles and locations would be best to apply to.
