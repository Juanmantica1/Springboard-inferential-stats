# Springboard-inferential-stats
Examining Racial Discrimination in the US Job Market
Background
Racial discrimination continues to be pervasive in cultures throughout the world. Researchers examined the level of racial discrimination in the United States labor market by randomly assigning identical résumés to black-sounding or white-sounding names and observing the impact on requests for interviews from employers.
Data
In the dataset provided, each row represents a resume. The 'race' column has two values, 'b' and 'w', indicating black-sounding and white-sounding. The column 'call' has two values, 1 and 0, indicating whether the resume received a call from employers or not.
Note that the 'b' and 'w' values in race are assigned randomly to the resumes when presented to the employer.
In [ ]:

​
Exercises
You will perform a statistical analysis to establish whether race has a significant impact on the rate of callbacks for resumes.
Answer the following questions in this notebook below and submit to your Github account.
What test is appropriate for this problem? Does CLT apply?
What are the null and alternate hypotheses?
Compute margin of error, confidence interval, and p-value.
Write a story describing the statistical significance in the context or the original problem.
Does your analysis mean that race/name is the most important factor in callback success? Why or why not? If not, how would you amend your analysis?
You can include written notes in notebook cells using Markdown:
In the control panel at the top, choose Cell > Cell Type > Markdown
Markdown syntax: http://nestacms.com/docs/creating-content/markdown-cheat-sheet
Resources
Experiment information and data source: http://www.povertyactionlab.org/evaluation/discrimination-job-market-united-states
Scipy statistical methods: http://docs.scipy.org/doc/scipy/reference/stats.html
Markdown syntax: http://nestacms.com/docs/creating-content/markdown-cheat-sheet
In [64]:

import pandas as pd
import numpy as np
import scipy.stats as st
In [2]:

data = pd.io.stata.read_stata('data/us_job_market_discrimination.dta')
In [3]:

# number of callbacks for black-sounding names
sum(data[data.race=='b'].call)
Out[3]:
157.0
In [15]:

df = pd.DataFrame([data.call,data.race]).transpose()
df.head() #Created a new data frame to better analyze call and race
​
Out[15]:
call	race
0	0	w
1	0	w
2	0	b
3	0	b
4	0	w
In [14]:

w=len(df[df.race=='w'])#Computing sample size for both races
b=len(df[df.race=='b'])
w,b
Out[14]:
(2435, 2435)
What test is appropriate for this problem? Does CLT apply?
The test that applies for this data is the z-statistic test because we are comparing between two proportions and because the sample that we are comparing is significantly large, more than>30.
The Central Limit Theorem applies because the sample size is large enough.
2.What are the null and alternate hypotheses?
Our null hypothesis will be that race does not affect call back ratio, and our alternate hypothesis will be that race does impact call back ratio
Compute margin of error, confidence interval, and p-value
In [81]:

#then we get the population proportions and substract them in order to compare the population proportions
wcm=df[df.race=='w']
wcm=wcm['call'].mean()
​
bcm=df[df.race=='b']
bcm=bcm['call'].mean()
ocm=wcm-bcm
ocm
Out[81]:
0.032032854209445585
In [57]:

#First we get the variance of black and white men that get call-backs
wcv=df[df.race=='w']
wcv=wcv['call'].var()
​
bcv=df[df.race=='b']
bcv=bcv['call'].var()
bcv,wcv
Out[57]:
(0.064476386036960986, 0.096509240246406572)
In [47]:

#Then we get the standard deviation 
std=(bcv+wcv)**(0.5)
std
Out[47]:
0.38415490914623895
In [70]:

#We are going to use a two tailed z-statistic to determine our confidence interval and our margin of error
z_stat=st.norm.ppf(.975)
lme=ocm-z_stat*std #low margin of error
hme=ocm+z_stat*std #high margin of error
lme, hme
Out[70]:
(-0.72089693220143936, 0.7849626406203305)
We are 95% confident that for our population mean must lie within -0.72089693220143936, 0.7849626406203305
In [88]:

Null=0
​
#first we get the mean proportion assuming our null hypotheses is 0 
p=df['call'].mean()
p
#Then our standard deviation
sd=(2.0*p*(1-p))/(len(df['call']))
sd=sd**.5
sd,p
Out[88]:
(0.0055132366451690808, 0.080492813141683772)
In [81]:

#then we get the population proportions and substract them in order to compare the population proportions
wcm=df[df.race=='w']
wcm=wcm['call'].mean()
​
bcm=df[df.race=='b']
bcm=bcm['call'].mean()
ocm=wcm-bcm
ocm
Out[81]:
0.032032854209445585
In [84]:

#We then compute the z-score
z_score=(ocm-0)/sd
z_score
Out[84]:
2.6328403330648102
In [92]:

#Finally we get the probaility or the p-value of getting that z-score
p=1-st.norm.cdf(z_score)
p
Out[92]:
0.0042337071338514054
In [93]:

1-p
Out[93]:
0.99576629286614859
4--Write a story describing the statistical significance in the context or the original problem.
The importance of the problem which we are adressing is that of racial descrimination in that of 21th century america. From the data obtained by the researches we are 99.6% confident that there is a impact of racial discrmination when it comes to job searching for black americans
Does your analysis mean that race/name is the most important factor in callback success? Why or why not? If not, how would you amend your analysis?
No. Because so far I have only analyzed race to callback success. To research if in fact race is the most important factor in I would need to run correlations between call back success and all the other factors.
Type Markdown and LaTeX: α2α2
