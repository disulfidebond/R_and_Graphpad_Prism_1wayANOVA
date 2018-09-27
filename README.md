## Common Statistical Tests in R and and Graphpad Prism: 1way-ANOVA

### Synopsis
ANOVA is short for ANalysis Of VAriance, and tests if the difference in variance among datapoints is statistically significant.  It is [primarily designed to work with normal data](https://stats.idre.ucla.edu/other/mult-pkg/whatstat/what-is-the-difference-between-categorical-ordinal-and-interval-variables/).  If you think you may be using the wrong test, [see this useful reference](https://stats.idre.ucla.edu/other/mult-pkg/whatstat/), or the attached PDF "Categorical_Data.PDF".

### Example Data: Samples A,B,C with expression values
      
        # SampleData
        A,B,C
        1,3,4
        2,4,5
        1,2,6
        2,1,4
        4,2,3

### One-Way ANOVA using Graphpad® Prism©
1) Open Graphpad Prism.  Select columns, and then select "Enter Replicate Values, stacked into columns".
2) Copy and paste, or enter the values manually into the spreadsheet that opens.
3) Click "Analyze", then select "One-Way Anova (and nonparametric)"
4) Select the options:
* No matching or pairing 
* Assume Gaussian Distribution: Yes. Use ANOVA
* Under "Multiple Comparisons, select "Compare the mean of each column with the mean of every other column"
* Under "Options", select Tukey.  If you wish, you can select additional options, but they are not required.
5) You should see this as the output:
        
        "Table Analyzed"	"Data 1"				
        "Data sets analyzed"	"A : A"	"B : B"	"C : C"		
        					
        "ANOVA summary"					
        "  F"	6.049				
        "  P value"	0.0152				
        "  P value summary"	*				
        "  Significant diff. among means (P < 0.05)?"	Yes				
        "  R square"	0.502				
        					
        "Brown-Forsythe test"					
        "  F (DFn, DFd)"	"1 (2, 12)"				
        "  P value"	0.3966				
        "  P value summary"	ns				
        "  Are SDs significantly different (P < 0.05)?"	No				
        					
        "Bartlett's test"					
        "  Bartlett's statistic (corrected)"	0.02495				
        "  P value"	0.9876				
        "  P value summary"	ns				
        "  Are SDs significantly different (P < 0.05)?"	No				
        					
        "ANOVA table"	SS	DF	MS	"F (DFn, DFd)"	"P value"
        "  Treatment (between columns)"	16.53	2	8.267	"F (2, 12) = 6.049"	P=0.0152
        "  Residual (within columns)"	16.4	12	1.367		
        "  Total"	32.93	14			
        					
        "Data summary"					
        "  Number of treatments (columns)"	3				
        "  Number of values (total)"	15				

And this for the multiple comparisons:

        "Number of families"	1							
        "Number of comparisons per family"	3							
        Alpha	0.05							
        								
        "Tukey's multiple comparisons test"	"Mean Diff."	"95.00% CI of diff."	Significant?	Summary	"Adjusted P Value"			
        								
        "  A vs. B"	-0.4	"-2.373 to 1.573"	No	ns	0.8529	A-B		
        "  A vs. C"	-2.4	"-4.373 to -0.4275"	Yes	*	0.0178	A-C		
        "  B vs. C"	-2	"-3.973 to -0.02746"	Yes	*	0.0468	B-C		
        								
        								
        "Test details"	"Mean 1"	"Mean 2"	"Mean Diff."	"SE of diff."	n1	n2	q	DF
        								
        "  A vs. B"	2	2.4	-0.4	0.7394	5	5	0.7651	12
        "  A vs. C"	2	4.4	-2.4	0.7394	5	5	4.591	12
        "  B vs. C"	2.4	4.4	-2	0.7394	5	5	3.825	12

Optional Challenge questions:
1) Does this dataset contain replicates?  
2) Is the data *actually* normally distributed?  How can you check for this, and if it is not normally distributed, how should you modify the analysis?

### One-Way ANOVA using R
* Preliminaries
R can do a lot of the analysis for you, but you *must* format the data properly.  This can either be done within R, in Excel, or by manually entering the values.

To run an ANOVA in R, you must provide R the values as well as the grouping (in this case, the factor) for the values.
This means this data:
A,B,C
1,3,4
2,4,5
1,2,6
2,1,4
4,2,3

Must be formatted like this, if you wish you can assign the column names of 'Sample' and 'Value' as headers:
A,1
A,1
A,2
A,2
A,4
B,1
B,2
B,2
B,3
B,4
C,3
C,4
C,4
C,5
C,6

* Analysis
There are several ways to do this; here is one possibility using the attached file "dataset4R.csv"

        rData <- reac.csv('dataset4R.csv')
        # format the dataframe by using the data.frame() command to create a new dataframe from the old one
        # the syntax is: data.frame(firstColumnValue=c(columnNames...), secondColumnValue=cbind(c(column1, column2, column3)))
        anovaDF <- data.frame(Groups=c("A", "A", "A", "A", "A", "B", "B", "B", "B", "B", "C", "C", "C", "C", "C"),Values=cbind(c(rData[,"A"], rData[,"B"], rData[,"C"])))
        anovaResults <- aov(Values~Groups,data=anovaDF)

        # display Anova results
        summary(anovaResults)


                    Df Sum Sq Mean Sq F value Pr(>F)  
        Groups       2   13.5   6.750    4.19 0.0518 .
        Residuals    9   14.5   1.611                 
        ---
        Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1


        # run Tukey's test
        tTest = TukeyHSD(anovaResults, ordered = TRUE)
        tTest

        Tukey multiple comparisons of means
          95% family-wise confidence level
          factor levels have been ordered

        Fit: aov(formula = Values ~ Groups, data = r1)

        $Groups
            diff         lwr      upr     p adj
        B-A  0.4 -1.57253595 2.372536 0.8528915
        C-A  2.4  0.42746405 4.372536 0.0178296
        C-B  2.0  0.02746405 3.972536 0.0468351
        
Optional Challenge questions:
3) Were the same tests used in Graphpad Prism and R?
4) Is there a simpler way in R to list multiple replicated values, instead of writing out "A", "A", "A",... etc?
