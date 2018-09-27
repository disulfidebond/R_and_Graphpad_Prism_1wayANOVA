Challenge Question answers:
1) Yes.  The replicates are stacked into columns, meaning each replicate for samples A,B, and C are a row value.  If this setup does not match how you setup your experiment, you should use a different configuration.
2) Yes, the data is normally distributed.  You can check this by plotting the data to determine if it fits a normal curve, or selecting "Analyze", "Column Statistics", and then running the Shapiro-Wilk Normality test.
[See this very good explanation of what the test describes](https://www.graphpad.com/guides/prism/7/statistics/index.htm?stat_howto_columnstatistics.htm), and [also see here](https://www.graphpad.com/guides/prism/7/statistics/index.htm?stat_howto_columnstatistics.htm) for important notes on running normality tests.
3) Yes.  You can find more information on the aov() and TukeyHSD() [commands](https://www.rdocumentation.org/packages/stats/versions/3.5.1/topics/aov) at the R reference site, or by selecting "Help" within R Studio.  If the data was not normally distributed, you would select "No, use non-parametric test" in the "Experimental Design" window.
4) Yes, with the rep() command:

        The syntax for the repeat command is:
        # c(rep("SomeValue", numberOfTimesToRepeat))
        anovaDF <- data.frame(Groups=c(rep("A", 4)), c(rep("B", 4)), c(rep("C", 4))),Values=cbind(c(r[,"A"], r[,"B"], r[,"C"])))
