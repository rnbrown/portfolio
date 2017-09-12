Terrorism Dataset Methodology

	The goal of this project was to draw conclusions from the Global Terrorism Database (GTD), a database on all  known terror attacks worldwide. Each row in the database corresponds with a single terror attack and has a wide array of information on that attack. Attacks were classified into 9 main types: 

Bombing/Explosion                      83073
Armed Assault                          40223
Assassination                          18402
Hostage Taking (Kidnapping)            10233
Facility/Infrastructure Attack          9581
Unknown                                 6425
Unarmed Assault                          913
Hostage Taking (Barricade Incident)      902
Hijacking                                598

	The GTD covers terror attacks from 1970-2016. There are 135 columns in this database that cover a wide array of information on each attack. However, this analysis focuses only on the date, location, and type of attack. The GTD is well-maintained, and required no cleaning on my part for the columns I wished to study. 
	
PART 2: Bayesian Inference

	The goal of this section was to compare the number of deaths in Iraq from terror attacks in 2007-2011 to 2012-2016. Specifically, I compared the distributions of mean and standard deviation of this statistic across the two groups to see if there was a distinct difference between these two time periods. To do this Bayesian inference, I used the pymc3 library.

	To start, I isolated only the terror attacks that occurred in Iraq within the time periods specified above. I grouped my data into 1-month periods, and I summed the number of deaths that occurred in each group to generate a pandas series. I wrote a function to take bootstrap samples from this series. I then used this function to generate an array of bootstrapped means and an array of bootstrapped standard deviations which I used to inform my prior distributions for these two statistics. 

	For the prior distribution of the mean, I assumed a normal distribution. The Central Limit Theorem states that as the sample size increases, the sampling distribution of the mean approaches a normal distribution. A rule of thumb is that with a sample size of at least 30, you can safely assume normality. In this case, I had data for 12 months per year x 10 years, so using a normal distribution for the prior is a very safe assumption. I generated that distribution using the mean and standard deviation of my bootstrapped means.

	I also generated a normal distribution using the mean and standard deviation of my bootstrapped standard deviations. I set that as the prior distribution of the standard deviation. The mean of this distribution was 5.4 standard deviations above 0, so it should be safe to use it as the prior without adjusting to account for the range of the standard deviation statistic not including any negative numbers. 

	Once I set my priors, I created a normal distribution for 2007-2011 and 2012-2016, and set the mean and standard deviations equal to my prior distributions. I updated each of these distributions with the corresponding data, and used the .sample() method in pymc3 to generate the posterior distributions. I then compared the two posterior distributions to see if they were significantly different from each other. 
	
	The posterior distributions for 2007-2011 and 2012-2016 were very significantly different from each other. The posterior distribution of the mean for 2007-2011 had a peak of about 300, while the posterior distribution of the mean for 2012-2016 had a peak of approximately 640. There was no overlap between the 95% credible intervals for these two populations. This tells us that there is some driving force causing the number of deaths due to terrorism in 2012-2016 to skyrocket as compared to the previous 5 years.

PART 3: Imputing Data

	In this section of the project, my goal was to impute the number of attacks in each category for each month of 1993. To do this, I created a new dataframe where each column corresponded with a single attack type. I aggregated the original data by month for all data before 1993, found the number of attacks of each type within that month, and stored the results for each separate month as a row in the new dataframe. I then fit a seasonal ARIMA model to each column and used that model to forecast the next 12 entries.

	In order to determine the best parameters for the seasonal ARIMA model, I first aggregated all the data by month without distinguishing between different categories of attacks. I created a single column with the total number of terror attacks worldwide. I ran a Dicky-Fuller test on this series to test for stationarity and discovered that there was a very clear upward trend. Therefore the data as is would be unsuitable for fitting a standard ARIMA model. I then created a column with the three month differences for these columns  and a column with the 12 month differences. Running the Dicky-Fuller on these columns gave me a test statistic less than the 1% critical value, meaning that I could fit a seasonal autoregressive model reasonable well using a 3 month season or a 12 month season. The 3-month differences gave me the lowest test statistic, so I settled on fitting a SARIMAX model with a 3-month season. 
