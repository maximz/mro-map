# Data sources

## Form 990 summaries (one CSV per year)

Download from https://www.irs.gov/statistics/soi-tax-stats-annual-extract-of-tax-exempt-organization-financial-data

We can filter down to the MROs through the "nonpfrea" column, which stands for "Reason for non-PF status". Value 05 means "Medical research organization (described in 170(b)1)(A)(i))”.

Here's a glimpse at a few rows of this data, along with the codebook's column descriptions: https://docs.google.com/spreadsheets/d/1g37FcUYw_TG_6pv5zA53nNRDLRTSN0ngpJQVpaGIB2o/edit?usp=sharing

The codebook/reference manual for this dataset is at https://www.irs.gov/pub/irs-soi/16eofinextractdoc.xls

Apparently some organizations have missing 990s for some fiscal years, so we need to look across several years of 990s to get a list of all MROs. We can compare our extracted list to another list Jeff shared (though I didn't catch its source): https://docs.google.com/spreadsheets/d/1FE82tYg-WIgUvnjA70y-z9Cas29Hqa2rjnoVfv9qLVE/edit?usp=drive_web. 

To get mapping of EIN to organization name and details: https://www.irs.gov/charities-non-profits/exempt-organizations-business-master-file-extract-eo-bmf (https://www.irs.gov/pub/irs-soi/eo_xx.csv)

Definitions of the program service revenue codes:

* homepage: https://www.census.gov/eos/www/naics/
* excel version: https://www.census.gov/eos/www/naics/2017NAICS/2017_NAICS_Structure.xlsx
* documentation: https://www.census.gov/eos/www/naics/2017NAICS/2017_NAICS_Manual.pdf

## Individual electronic Form 990s on AWS (one XML per return)

* XML format.
* Includes detailed breakdown of revenue sources.
* IRS press release: https://www.irs.gov/newsroom/irs-makes-electronically-filed-form-990-data-available-in-new-format
* AWS public dataset listing: https://aws.amazon.com/public-datasets/irs-990/
* XML schema (doesn't appear to be downloadable): https://www.irs.gov/e-file-providers/current-valid-xml-schemas-and-business-rules-for-exempt-organizations-modernized-e-file

Index of all files is available at: `s3://irs-form-990/index_2016.csv` (see other years by doing: `s3cmd ls s3://irs-form-990-/index`).

A third-party guide to how to use this data source: https://www.reddit.com/r/aws/comments/4p772f/how_the_heck_do_i_view_the_990_documents_on/.
e.g. https://irs-xml-search.herokuapp.com/search?name=jackson%laboratory

Here's an excerpt from one return (https://s3.amazonaws.com/irs-form-990/201110749349300811_public.xml):

```
<ProgramServiceRevenue>
	<Description>GENETIC RESOURCES</Description>
	<BusinessCode>900099</BusinessCode>
	<TotalRevenueColumn>126788661</TotalRevenueColumn>
	<RelatedOrExemptFunctionIncome>126788661</RelatedOrExemptFunctionIncome>
</ProgramServiceRevenue>
<ProgramServiceRevenue>
	<Description>TRAINING & EDUCATION</Description>
	<BusinessCode>900099</BusinessCode>
	<TotalRevenueColumn>554462</TotalRevenueColumn>
	<RelatedOrExemptFunctionIncome>554462</RelatedOrExemptFunctionIncome>
</ProgramServiceRevenue>
<ProgramServiceRevenue>
	<Description>OTHER PROGRAM INCOME</Description>
	<BusinessCode>900099</BusinessCode>
	<TotalRevenueColumn>274913</TotalRevenueColumn>
	<RelatedOrExemptFunctionIncome>274913</RelatedOrExemptFunctionIncome>
</ProgramServiceRevenue>
<TotalProgramServiceRevenue>127618036</TotalProgramServiceRevenue>
```

Scripts to parse: https://github.com/slibby/irs-990-parse

Charity Navigator's decoder of this data: http://990.charitynavigator.org/ (deprecated); https://github.com/CharityNavigator/990_long (updated version); based on concordance file https://github.com/Nonprofit-Open-Data-Collective/irs-efile-master-concordance-file

## ProPublica nonprofit data API

* API documentation: https://projects.propublica.org/nonprofits/api
* Here's an example of their available data: https://projects.propublica.org/nonprofits/organizations/10211513


## NIH awards

* Online browser: https://www.report.nih.gov/award/index.cfm
* Downloadable export: https://exporter.nih.gov/ExPORTER_Catalog.aspx
* e.g. 2016: https://exporter.nih.gov/CSVs/final/RePORTER_PRJ_C_FY2016.zip

## NCCS core trend files

Julia Lane also pointed us to NCCS core trend files (http://nccs-data.urban.org/index.php), which seem to be compilations of the 990s and may prove useful.

