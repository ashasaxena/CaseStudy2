# CaseStudyTwo
Jim Park Asha Saxena Andrew Walch  
November 25, 2017  




#2a Read the csv into R and take a look at the data set. Output how many rows and columns the data.frame is.


```r
PD <- read.csv("Procrastination.csv")
dim(PD) #results in printing the number of rows then the number of columns.
```

```
## [1] 4264   61
```


#2b The column names are either too much or not enough. Change the column names so that they do not have spaces, underscores, slashes, and the like. All column names should be under 12 characters. Make sure you’re updating your codebook with information on the tidied data set as well.


```r
class(PD)
```

```
## [1] "data.frame"
```

```r
OldNames <- names(PD)
library(data.table)
PD.renamed <- setnames(PD, old=OldNames, new=c("Age", "Gender", "Kids", "Education","WorkStatus","AnnualIncome", "Occupation", "Years", "Months", "Community", "Country", "Status", "NumSon", "NumDaught", "DP1", "DP2","DP3","DP4","DP5","AIP1","AIP2","AIP3","AIP4","AIP5","AIP6","AIP7","AIP8","AIP9","AIP10","AIP11","AIP12","AIP13","AIP14","AIP15", "GP1", "GP2", "GP3", "GP4", "GP5", "GP6", "GP7", "GP8", "GP9", "GP10", "GP11", "GP12", "GP13", "GP14", "GP15", "GP16", "GP17", "GP18", "GP19", "GP20", "SWLS1", "SWLS2", "SWLS3", "SWLS4", "SWLS5","Q1Self","Q2Others"))
head(PD.renamed)
```

```
##    Age Gender     Kids Education WorkStatus AnnualIncome Occupation
## 1 67.5   Male Yes Kids        ma    retired        25000           
## 2 45.0   Male Yes Kids       deg  part-time        35000           
## 3 19.0 Female  No Kids       dip    student           NA           
## 4 37.5   Male Yes Kids        ma  full-time        45000           
## 5 28.0 Female  No Kids       deg  full-time        35000           
## 6 23.0 Female  No Kids       deg  full-time        15000           
##     Years Months  Community        Country   Status NumSon NumDaught DP1
## 1 9.0e+00      0 Large-City    El Salvador Divorced      0         5   3
## 2 1.5e-19      0    Village        Bolivia  Married   Male         1   3
## 3 0.0e+00      0 Large Town         Cyprus   Single      0         0   5
## 4 1.4e+01      0 Large Town Czech Republic  Married      0         1   3
## 5 1.0e+00      0    Village Czech Republic   Single      0         0   3
## 6 1.0e+00      0 Small Town Czech Republic   Single      0         0   3
##   DP2 DP3 DP4 DP5 AIP1 AIP2 AIP3 AIP4 AIP5 AIP6 AIP7 AIP8 AIP9 AIP10 AIP11
## 1   1   1   1   1    1    1    1    1    1    1    1    1    5     1     1
## 2   4   3   3   3    3    1    4    3    3    4    3    3    3     3     4
## 3   5   2   3   3    5    4    4    5    5    5    5    4    5     5     4
## 4   3   3   3   3    2    1    4    3    5    3    4    5    4     5     4
## 5   3   2   1   1    1    1    3    3    2    2    2    2    1     1     2
## 6   4   3   2   2    2    5    5    5    5    3    5    4    4     5     3
##   AIP12 AIP13 AIP14 AIP15 GP1 GP2 GP3 GP4 GP5 GP6 GP7 GP8 GP9 GP10 GP11
## 1     1     1     1     3   1   1   1   1   1   1   1   1   1    1    5
## 2     2     2     2     4   4   2   2   2   2   2   4   2   4    2    3
## 3     3     5     4     3   5   2   2   4   3   1   3   2   5    4    5
## 4     3     4     2     1   4   1   3   3   2   3   4   5   4    1    3
## 5     1     2     1     2   4   1   2   4   5   2   4   2   4    1    2
## 6     5     4     5     5   5   5   2   5   4   4   5   4   4    3    4
##   GP12 GP13 GP14 GP15 GP16 GP17 GP18 GP19 GP20 SWLS1 SWLS2 SWLS3 SWLS4
## 1    1    1    1    1    1    1    5    1    5     5     5     5     5
## 2    4    2    2    3    4    3    3    4    4     3     4     4     4
## 3    5    3    4    5    2    3    5    5    4     2     2     2     3
## 4    4    3    3    4    4    3    4    5    1     2     4     2     2
## 5    3    2    4    3    2    3    2    3    4     4     4     4     3
## 6    4    3    4    4    4    4    4    4    4     3     2     4     4
##   SWLS5 Q1Self Q2Others
## 1     5     no       no
## 2     3    yes      yes
## 3     4    yes      yes
## 4     2    yes      yes
## 5     4     no       no
## 6     3    yes      yes
```

#2c. Some columns are, due to Qualtrics, malfunctioning. Prime examples are the following columns: “How long have you held this position?: Years”, Country of residence, Number of sons, and Current Occupation.

i Some have impossible data values. Detail what you are doing to fix these columns in the raw data and why. It’s a judgment call for each, but explain why. For example, most people have not been doing anything for over 100 years. For the “Years” columns, round to the nearest integer.

ii Somehow, “Number of sons” was labeled with Male (1) and Female (2). Change these incorrect labels back to integers.

iii There are no “0” country of residences. Treat this as missing.

iv Current Occupation has no “please specify” or “0.” Treat them as missing. Some jobs are quite similar. Use judgment calls to make overwrite them into the same category. It does not have to be 100% accurate, but right now “ESL Teacher” would not be counted as “teacher” if there were unique counts.

#2Ci. Fix impossible data values.  Round Age and Years to nearest integer (negative exponents will default to 0 due to this), Correct Years '999' entries and treat as missing(NA), and correct number of sons entries where Male = 1 sons, and Female =2 sons, Change 0 in "Country" to NA to indicate the data is missing, change "please specify" or "0" under Occupation to NA to indicate missing, and combine similar occupations into category of occupation.

```r
PD.renamed$Age <- as.integer(round(PD.renamed$Age)) #rounds all Ages to the nearest integer and converts from numeric to integer.
PD.renamed$Years <- as.integer(round(PD.renamed$Years)) #rounds all Years to the nearest integer and converts from from numeric to integer.
PD.renamed$Years <- as.integer(gsub(999, NA, PD.renamed$Years)) #Removes 999 values and treats them as missing by indicating with an NA
PD.renamed$NumSon <- (gsub("Male", 1, PD.renamed$NumSon)) #subs a 1 where the word Male is listed in NumSon
PD.renamed$NumSon <- (gsub("Female", 2, PD.renamed$NumSon)) #subs a 2 where the word Female is listed in NumSon
PD.renamed$NumSon <- as.integer(PD.renamed$NumSon) #Converts NumSon to an integer.
PD.renamed$Country <- gsub(0, NA, PD.renamed$Country) #displays Country with 0 as missing data (NA)
PD.renamed$Country[PD.renamed$Country == ""] <- NA #replaces all non-entries with NA
PD.renamed$Occupation[PD.renamed$Occupation == 0] <- NA
PD.renamed$Occupation[PD.renamed$Occupation == "please specify"] <- NA
PD.renamed$Occupation[PD.renamed$Occupation == ""] <- NA
```
#2e.  Each variable that starts with either DP, AIP, GP, or SWLS is an individual item on a scale. For example, DP 1 through DP 5 are five different questions on the Decision Procrastination Scale. I’ve reverse-scored them for you already, but you should create a new column for each of them with their mean. To clarify, you’ll need a DPMean column, an AIPMean column, a GPMean column, and a SWLSMean column. This represents the individual’s average decisional procrastination (DP), procrastination behavior (AIP), generalized procrastination (GP), and life satisfaction (SWLS).



```r
PD.renamed$DPMean=rowMeans(PD.renamed[,15:19]) #Creates column for mean of all DP entries in observation.
PD.renamed$AIPMean=rowMeans(PD.renamed[,20:34]) #Creates column for mean of all AIP entries in observation.
PD.renamed$GPMean=rowMeans(PD.renamed[,35:54]) #Creates column for mean of all GP entries in observation.
PD.renamed$SWLSMean=rowMeans(PD.renamed[,55:59]) #Creates column for mean of all SWLS entries in observation.
PD.renamed[,62:65] <- round(PD.renamed[,62:65], digits = 1) #Round DP, AIP, GP, and SWLS means to 1 digit.
head(PD.renamed[,62:65])
```

```
##   DPMean AIPMean GPMean SWLSMean
## 1    1.4     1.4    1.6      5.0
## 2    3.2     2.9    2.9      3.6
## 3    3.6     4.4    3.6      2.6
## 4    3.0     3.3    3.2      2.4
## 5    2.0     1.7    2.8      3.8
## 6    2.8     4.3    4.0      3.2
```

##3 SCRAPING
#3a Scraped Country and HDI

```r
library('rvest') # grab and parse HTML
```

```
## Loading required package: xml2
```

```r
#Specifying the url for desired website to be scrapped
url <- 'https://en.wikipedia.org/wiki/List_of_countries_by_Human_Development_Index#Complete_list_of_countries'

webpage <- read_html(url)#Reading the HTML code from the website

#Using XPath selectors to scrap the Country and HDI of first 8 tables
Country<-webpage%>%html_nodes("h3+ div .wikitable:nth-child(1) td:nth-child(3)")%>%html_text()
HDI<-webpage%>%html_nodes("h3+ div td:nth-child(4)")%>%html_text()

###  WILL NEED TO FIX "WORLD" ENTRY
CountryHDI <- data.frame(Country, HDI)#Convert to data frame
CountryHDI
```

```
##                                Country   HDI
## 1                               Norway 0.949
## 2                            Australia 0.939
## 3                          Switzerland 0.939
## 4                              Germany 0.926
## 5                              Denmark 0.925
## 6                            Singapore 0.925
## 7                          Netherlands 0.924
## 8                              Ireland 0.923
## 9                              Iceland 0.921
## 10                              Canada 0.920
## 11                       United States 0.920
## 12                           Hong Kong 0.917
## 13                         New Zealand 0.915
## 14                              Sweden 0.913
## 15                       Liechtenstein 0.912
## 16                      United Kingdom 0.909
## 17                               Japan 0.903
## 18                         South Korea 0.901
## 19                              Israel 0.899
## 20                          Luxembourg 0.898
## 21                              France 0.897
## 22                             Belgium 0.896
## 23                             Finland 0.895
## 24                             Austria 0.893
## 25                            Slovenia 0.890
## 26                               Italy 0.887
## 27                               Spain 0.884
## 28                      Czech Republic 0.878
## 29                              Greece 0.866
## 30                              Brunei 0.865
## 31                             Estonia 0.865
## 32                             Andorra 0.858
## 33                              Cyprus 0.856
## 34                               Malta 0.856
## 35                               Qatar 0.856
## 36                              Poland 0.855
## 37                           Lithuania 0.848
## 38                               Chile 0.847
## 39                        Saudi Arabia 0.847
## 40                            Slovakia 0.845
## 41                            Portugal 0.843
## 42                United Arab Emirates 0.840
## 43                             Hungary 0.836
## 44                              Latvia 0.830
## 45                           Argentina 0.827
## 46                             Croatia 0.827
## 47                             Bahrain 0.824
## 48                          Montenegro 0.807
## 49                              Russia 0.804
## 50                             Romania 0.802
## 51                              Kuwait 0.800
## 52                             Belarus 0.796
## 53                                Oman 0.796
## 54                            Barbados 0.795
## 55                             Uruguay 0.795
## 56                            Bulgaria 0.794
## 57                          Kazakhstan 0.794
## 58                             Bahamas 0.792
## 59                            Malaysia 0.789
## 60                               Palau 0.788
## 61                              Panama 0.788
## 62                 Antigua and Barbuda 0.786
## 63                          Seychelles 0.782
## 64                           Mauritius 0.781
## 65                 Trinidad and Tobago 0.780
## 66                          Costa Rica 0.776
## 67                              Serbia 0.776
## 68                                Cuba 0.775
## 69                                Iran 0.774
## 70                             Georgia 0.769
## 71                              Turkey 0.767
## 72                           Venezuela 0.767
## 73                           Sri Lanka 0.766
## 74               Saint Kitts and Nevis 0.765
## 75                             Albania 0.764
## 76                             Lebanon 0.763
## 77                              Mexico 0.762
## 78                          Azerbaijan 0.759
## 79                              Brazil 0.754
## 80                             Grenada 0.754
## 81              Bosnia and Herzegovina 0.750
## 82                           Macedonia 0.748
## 83                             Algeria 0.745
## 84                             Armenia 0.743
## 85                             Ukraine 0.743
## 86                              Jordan 0.741
## 87                                Peru 0.740
## 88                            Thailand 0.740
## 89                             Ecuador 0.739
## 90                               China 0.738
## 91                                Fiji 0.736
## 92                            Mongolia 0.735
## 93                         Saint Lucia 0.735
## 94                             Jamaica 0.730
## 95                            Colombia 0.727
## 96                            Dominica 0.726
## 97                            Suriname 0.725
## 98                             Tunisia 0.725
## 99                  Dominican Republic 0.722
## 100   Saint Vincent and the Grenadines 0.722
## 101                              Tonga 0.721
## 102                              World 0.717
## 103                              Libya 0.716
## 104                             Belize 0.706
## 105                              Samoa 0.704
## 106                           Maldives 0.701
## 107                         Uzbekistan 0.701
## 108                            Moldova 0.699
## 109                           Botswana 0.698
## 110                              Gabon 0.697
## 111                           Paraguay 0.693
## 112                              Egypt 0.691
## 113                       Turkmenistan 0.691
## 114                          Indonesia 0.689
## 115                          Palestine 0.684
## 116                            Vietnam 0.683
## 117                        Philippines 0.682
## 118                        El Salvador 0.680
## 119                            Bolivia 0.674
## 120                       South Africa 0.666
## 121                         Kyrgyzstan 0.664
## 122                               Iraq 0.649
## 123                         Cape Verde 0.648
## 124                            Morocco 0.647
## 125                          Nicaragua 0.645
## 126                          Guatemala 0.640
## 127                            Namibia 0.640
## 128                             Guyana 0.638
## 129                         Micronesia 0.638
## 130                         Tajikistan 0.627
## 131                           Honduras 0.625
## 132                              India 0.624
## 133                             Bhutan 0.607
## 134                        Timor Leste 0.605
## 135                            Vanuatu 0.597
## 136             Congo, Republic of the 0.592
## 137                  Equatorial Guinea 0.592
## 138                           Kiribati 0.588
## 139                               Laos 0.586
## 140                         Bangladesh 0.579
## 141                              Ghana 0.579
## 142                             Zambia 0.579
## 143              São Tomé and Príncipe 0.574
## 144                           Cambodia 0.563
## 145                              Nepal 0.558
## 146                            Myanmar 0.556
## 147                              Kenya 0.555
## 148                           Pakistan 0.550
## 149                          Swaziland 0.541
## 150                              Syria 0.536
## 151                             Angola 0.533
## 152                           Tanzania 0.531
## 153                            Nigeria 0.527
## 154                           Cameroon 0.518
## 155                   Papua New Guinea 0.516
## 156                           Zimbabwe 0.516
## 157                    Solomon Islands 0.515
## 158                         Mauritania 0.513
## 159                         Madagascar 0.512
## 160                             Rwanda 0.498
## 161                            Comoros 0.497
## 162                            Lesotho 0.497
## 163                            Senegal 0.494
## 164                              Haiti 0.493
## 165                             Uganda 0.493
## 166                              Sudan 0.490
## 167                               Togo 0.487
## 168                              Benin 0.485
## 169                              Yemen 0.482
## 170                        Afghanistan 0.479
## 171                             Malawi 0.476
## 172                      Côte d'Ivoire 0.474
## 173                           Djibouti 0.473
## 174                             Gambia 0.452
## 175                           Ethiopia 0.448
## 176                               Mali 0.442
## 177  Congo, Democratic Republic of the 0.435
## 178                            Liberia 0.427
## 179                      Guinea Bissau 0.424
## 180                            Eritrea 0.420
## 181                       Sierra Leone 0.420
## 182                         Mozambique 0.418
## 183                        South Sudan 0.418
## 184                             Guinea 0.414
## 185                            Burundi 0.404
## 186                       Burkina Faso 0.402
## 187                               Chad 0.396
## 188                              Niger 0.353
## 189           Central African Republic 0.352
```

```r
## TODO need to fix "world" entry
```

