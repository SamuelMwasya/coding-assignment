# coding-assignment

#Question 1

```{r}
#Download the following file: 'https://malariagen-eu.s3.amazonaws.com/public/2012/05/codingtests/people.csv'

sdc <- read.table("https://malariagen-eu.s3.amazonaws.com/public/2012/05/codingtests/people.csv", sep="\t", header=TRUE)
sdc

#what is the total number of people?

length(people$gender)
#or
nrow(people)



#what countries are represented?


unique(people$country, incomparables = F )

#How many people are there from each country?

#library(janitor)
#tabyl(people, country, gender)
#tabyl(people, country)
library(dplyr)
people %>% group_by(country) %>% tally()

#mean age of men and females

tapply(people$age, people$gender, mean)
#or 
people %>% group_by(gender) %>% summarise(mean(age))

#compute the bmi for each patient. (weightin kg/ sq of height(m))

dy<-people %>% mutate(BMI = weight/height^2) %>% select(id, BMI) %>%write_csv("/cloud/project/idwithbmi.csv")

#make a scatterplot of height versus weight. what other factor is influencing these two variables.

y <- people$height
x <- people$weight
plot(x,y, main = "A scatter plot of height versus weight", xlab = "Height (m)", ylab = "Weight (kg)")
```


#Question 2
#downloading the file;
```{bash}
wget https://malariagen-eu.s3.amazonaws.com/public/2012/05/codingtests/snps.csv 
```

#The number of each type of nucleotide change
```{bash}
grep -v "chromosome" snps.csv | cut -f 3,4 | sort | uniq -c
```

#The number of transition and the number of transversions.
```{bash}
#We now make a bash script that takes the file name as its first and only argument.

nano transitions_transversions.sh

#the code below extracts the 3rd and the 4th if they are the one having the reference and alternate seq. fields of the file.
#and it combines the two columns into one columns and saves it to sams.csv file 

cut -f 3,4 $1 |grep -v "ref" |awk  '{print $1$2}'| cut -d " " -f 2  > sams.csv

#the number of transition are obtained by the following code and printed to the screen.
#A<->G AND C<->T
grep -c  -E "AG|GA|CT|TC" sams.csv > transitions.csv
echo "The number of transitions are:"  
cat transitions.csv

#the number of tranversions are obtained by using the following code and printed to the screen
grep -c  -E "AC|CA|TG|GT|AG|GA|TC|CT" sams.csv >transversions.csv
echo  "The number of transversions are:"  
cat transversions.csv
```

#the overall TiTv rate = no. of transition/no. of transversion
```{bash}
# no. of transition= A<->G and C<->T
transition=`grep -c  -E "AG|GA|CT|TC" sams.csv`
transversion=`grep -c  -E "AC|CA|TG|GT|AG|GA|TC|CT" sams.csv`
#to get TiTv rate we run
transition/tranversion
```



#Question 4

```{bash}
#this scripts once invoked it asks the user to input the gene they want to search.
#after entering the correct gene id it will display all relating genes.
nano web.sh

#!/bin/bash
echo Kindly, enter the gene name to retrieve
read gene
grep $gene snps_with_genes.csv | cat

bash web.sh <gene_name>
```

