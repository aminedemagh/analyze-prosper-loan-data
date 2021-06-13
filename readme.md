# Prosper Loan Data Exploration

## Date created
This project was created on 2021-05-22.

## Description

This project explores [Prosper Marketplace](https://www.prosper.com/)’s loans dataset. Prosper is a website for peer-to-peer lending. Borrowers fill in forms to ask for loans that are presented to potential investors on their webpage. The platform offers loans ranging from 2000$ to 40,000$. Investors receive yearly interest while Prosper [produces revenue](https://prosper.zendesk.com/hc/en-us/articles/210013193-How-does-Prosper-make-money-) from servicing fees and transaction fees.

The data can be downloaded from [here](https://s3.amazonaws.com/udacity-hosted-downloads/ud651/prosperLoanData.csv)

The dictionary to explain the variables can be downloaded from [here](https://docs.google.com/spreadsheets/d/1gDyi_L4UvIrLTEC6Wri5nbaMmkGmLQBk-Yx3z0XDEtI)

## Files used

- __prosper_exploration.ipynb__ : a jupyter notebook containing the code and the documentation for the analysis.
- __prosper_exploration_slides.ipynb__ : a subset of `prosper_exploration.ipynb`.  It contains only the main insights that were uncovered in the analysis and it is used to create a slideshow to display them.
- __prosper_exploration_slides.slides.html__ : the slides that were created from `prosper_exploration_slides.ipynb` using jupyter nbconvert. They can be viewed using any browser.
- __output_toggle.tpl__ : the template that was used to create the slides.

## Installation

To to run the notebook, you will need `jupyter`, `python`, `pandas`, `matplotlib`, and `seaborn` installed. You will also need `nbconvert` to convert your .ipynb file to slides.

You can download all these libraries and tools individually or with [Anaconda](https://www.anaconda.com/), a python distribution with a focus on data science. If you’re interested in Anaconda you can follow their [installation guide](https://www.anaconda.com/distribution/).

If you want to convert `prosper_exploration_slides.ipynb` to slides, you need to run the command below in your terminal :

`jupyter nbconvert prosper_exploration_slides.ipynb --to slides --post serve --template output_toggle`

This command will also start a server and load the slides in your default browser. However, you can use it only with a version of `nbconvert` that is earlier than 6. After 6, `nbconvert` stopped using .tpl files for templates and started using directories.

If you downloaded Anaconda recently, you're likely to have a version of `nbconvert` that is older than 6. Use the command below to download an earlier version :

`pip install 'nbconvert < 6.0'`

After converting your notebook to slides, you should upgrade `nbconvert` to the latest version to use all its functionalities (converting to .pdf, .html, ...)

`pip install --upgrade nbconvert`

## Dataset

The dataset that I chose was Prosper's loan dataset. It contains information about 113937 loans and 81 variables. It includes two periods of the company: a pre-2009 period before the enactment of the laws for online lending and a post-2009 period where Prosper changed its business model to comply with the new laws. 

I only selected the loans that originated after 2009 because I wanted to capture the latest trends in the data. My subset was comprised of 84997 loans that originated from 6 August 2009 to 27 March 2014.

I selected 9 columns from the dataset for my analysis: `ListingKey`, `listing_category_num`, `prosper_rating_alpha`, `LoanOriginalAmount`, `Term`, `LenderYield`, `LoanOriginationDate`, `ClosedDate` and `LoanStatus`.

The cleaning steps involved renaming some columns, deleting missing values, converting `listing_category_num` from numbers to category names, casting the datatype of `LoanOriginationDate` and `ClosedDate` to datetime, casting `prosper_rating_alpha`, `Term` and `LoanStatus` to categorical dtype.`Term` was ordered ascendingly (12-months term < 36-months term < 60-months term) and`prosper_rating_alpha` was ordred from the least risky investement to the riskiest (AA < A < B < C < D < E < HR (High Risk) ).



## Summary of Findings

I explored 6 variables: loan amount, listing category, loan term, lender yield, Prosper rating, and loan status. I wanted to know which variables could predict the loan status but I was only interested in the outcome of a loan (whether it was completed or charged-off). I plotted each of the variables to get an idea about their distributions. I discovered that 1/5 of the loans with an outcome were charged off. For the listing category, I found that 9 of the values represented almost all the data points with one particular category 'Debt Consolidation' that represented 62.68%. With this in mind, I divided the listing categories into two groups: one for common categories that represented at least 1% of the data and another group for rare categories that represented less than 1%. 

In the bivariate analysis, I found that the increase in the loan term, Prosper rating, and the lender yield increased the percentage/number of charged-off loans. I also noticed that each listing category was comprised of a different number of completed and charged-off loans. The surprise was to find that higher values for loan amount were associated with more completed loans while I expected bigger loans to be harder to pay. 

In addition, I found that all the variables interacted with Prosper rating (Prosper's risk score). An increase in Prosper rating was followed by an increase in the lender yield and a decrease in the loan amount. From AA to C, the percentage of 60-months term loans increased, and the percentage of 36- and 12-months term loans decreased but from C to HR the relationship was inverted. Each listing category had a different percentage of each Prosper rating. Some categories had a higher percentage of risky loans than others. 
Those findings suggested that the effects that I observed on loan status could be due indirectly to Prosper rating instead of the independent influence of my other variables.

To verify the effect of Prosper rating on my variables, I plotted each one of them with both Prosper rating and the number/percentage of charged-off loans. I found that for each Prosper Rating, the increase of the term increased the percentage of charged-off loans. For the lender yield, higher values were associated with more charged-off loans within each Prosper rating. I also discovered that the ranking of the categories according to the percentage of charged-off loans changed from a Prosper rating to another. If a category had the least percentage of charged-off loans in a rating, it could have the highest in another one. Finally, I found that the distribution of the loan amount for charged-off loans is higher than the distribution of completed loans within each Prosper rating until HR where they overlap.

To get an even deeper sense of the interactions between my variables, I decided to plot the lender yield, the term, Prosper rating, and the loan status together. I found that the term increased the starting point of the distribution of the lender yield but it did not influence its relationship with the loan status. Furthermore, I facetted my plot with both the term and the listing category. I noticed that each combination had a different pattern and that even within the same combination, the relationship between the lender yield and the loan status was not consistent across different Prosper ratings. 

## Key Insights for Presentation

I started my presentation by showing the distribution of the loan status using a bar chart. Then, I plotted its relationship with Prosper Rating using a grouped bar chart. After that, I wanted to show that the other variables (term, loan amount, lender yield, listing category) had an independent effect on the loan status. Therefore, I plotted each one of them with both Prosper Rating and the loan status. I used a line plot for the term and the listing categories and I used boxplots for the loan amount and the lender yield. I chose not to present the remaining bivariate plots because their relationships were already covered in the 3-variables plots. Also, I left the visualizations with 4 and 5 variables because they were hard to display and interpret. 

Compared to the exploration step, I changed the 'describe_cateogry()' function to return the axes object of the plots that it drew to customize them, I also removed the print statements from the function, and I formatted the y-axis of the percentages with PercentFormatter.
