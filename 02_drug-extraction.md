Extracting Requested Drugs
================
Michael Quinn Maguire, MS
2022-07-15

# Loading packages for cleaning and importing

``` r
library(tidyverse)
library(readxl)
```

# Examine the sheets in the excel file

``` r
phc_file_sheets <- readxl::excel_sheets(
  path = './data/raw/Aggregated data_tn_meds _version3.xlsx'
)

phc_file_sheets
```

    ##  [1] "gabapentinoids" "gabapentin"     "CBZ"            "OxCBZ"         
    ##  [5] "pregabaline"    "Baclofen"       "Duloxetine"     "Lamotrigine"   
    ##  [9] "Topiramate"     "Opioids"

-   PHC requested the following drugs:
    1.  Carbamazepine (CBZ)
    2.  Oxcarbazepine (OxCBZ)
    3.  Gabapentin (gabapentin)
    4.  Pregabalin (pregabaline)
    5.  Gabapentin and Pregabalin (gabapentinoids)
    6.  opioids (Opioids)

``` r
requested_drugs <- grep(phc_file_sheets, 
                        pattern = '(CBZ)|(Gabapentin)|(Pregabaline)|(Opioids)',
                        ignore.case = TRUE,
                        value = TRUE)

requested_drugs
```

    ## [1] "gabapentinoids" "gabapentin"     "CBZ"            "OxCBZ"         
    ## [5] "pregabaline"    "Opioids"

# Read in each file/sheet

``` r
requested_drugs_file <- do.call('rbind',
                                lapply(
                                  requested_drugs,
                                  FUN = function(x) readxl::read_excel(
                                    path = './data/raw/Aggregated data_tn_meds _version3.xlsx',
                                    sheet = x
                                    ) |> mutate(
                                      origin = x
                                      )
                                  )
                                )
```

# Write out the final file

``` r
write_csv(requested_drugs_file,
          file = './data/clean/tn-aggregates-combined.csv')
```
