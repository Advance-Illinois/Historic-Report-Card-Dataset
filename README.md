# Instructions for Running

Before running the jupyter notebook in this environment, you must create a local data folder in the same folder as the jupyter notebook and the `Local Historic Crosswalk.xlsx` and `Historic Report Card Processessing.ipynb` file. The data folder is not included here because the files inside are large, but the Data Folder Contents section below outlines the folder's contents, and all of the contents can be downloaded from ISBE's Report Card Data Library page.

## Data Folder Contents

Below are the contents of the data folder. All of these files are raw downloads from [ISBE's Report Card Data Library](https://www.isbe.net/Pages/Illinois-State-Report-Card-Data.aspx)

| Year | Layout Files     | Excel Report Card Data                | Related Data Files  |
| :--: | ---------------- | ------------------------------------- | ------------------- |
| 2008 | RC08_layout.xls  | rc08u.txt                             |                     |
| 2009 | RC09_layout.xls  | rc09.txt                              |                     |
| 2010 | RC10_layout.xls  | rc10.txt                              |                     |
| 2011 | RC11_layout.xls  | rc11u.txt                             |                     |
| 2012 | RC12-layout.xls  | rc12.txt                              |                     |
| 2013 | RC13_layout.xlsx | rc13.txt                              |                     |
| 2014 | RC14_layout.xlsx | rc14.txt                              |                     |
| 2015 | RC15-layout.xlsx | rc15.txt                              | rc15-assessment.txt |
| 2016 | RC16-layout.xlsx | rc16.txt                              | rc16_assessment.txt |
| 2017 | RC17_layout.xlsx | rc17.txt                              | rc17_assessment.txt |
| 2018 |                  | Report-Card-Public-Data-Set.xlsx      |                     |
| 2019 |                  | 2019-Report-Card-Public-Data-Set.xlsx |                     |
| 2020 |                  | 2020-Report-Card-Public-Data-Set.xlsx |                     |
| 2021 |                  | 2021-RC-Pub-Data-Set.xlsx             |                     |
| 2022 |                  | 2022-Report-Card-Public-Data-Set.xlsx |                     |
| 2023 |                  | 23-RC-Pub-Data-Set.xlsx               |                     |
| 2024 |                  | 24-RC-Pub-Data-Set.xlsx               |                     |
| 2025 |                  | 2025-Report-Card-Public-Data-Set.xlsx |                     |

# Adding a new year of data

Warning: These instructions may not apply if ISBE changes their data format for a new year. Without a change of the data format, there may be other changes that cause certain procedures to break. The latter case is unlikely, but definitely possible.

## 1. Including the New Report Card File

1. Got to [ISBE's Report Card Data Library](https://www.isbe.net/Pages/Illinois-State-Report-Card-Data.aspx) and download the new report card.

2. Move the file to the `data` folder

3. Update the `README.md` file table with the new report card year and file.

4. Update the `filename_crosswalk` dictionary with the new year as a key and the new filename as a value

5. If you know that any new demographics are used, add the demographic to the DEMOGRAPHICS list.

## 2. Updating the Crosswalk

Note: The Advance Illinois sharepoint file: `Historic Crosswalk.xlsx` should be kept in sync with `Local Historic Crosswalk.xlsx`. If you are updating `Historic Crosswalk.xlsx`, "the crosswalk file" will below refer to `Historic Crosswalk.xlsx`, and you should complete the last step. Otherwise, "the crosswalk file" will simply refer to `Local Historic Crosswalk.xlsx`.

1. Open the Crosswalk file to the `Report Card Metadata` sheet. Add a new row at the bottom for the new year of data.

2. Go to the `Name Crosswalk` sheet. Copy the previous year's data and insert it in a new table row. Make sure to change the year column to the new year. Do not make any other edits to the new row yet.

3. Go to the `Details` sheet. Sort by year (or filter to the previous year). Select all of the previous year's rows. Copy them to the table, and increment the year column for all the new rows.

4. Go through these rows one by one, comparing them to the headers in the newly downloaded report card file. Most rows should have accurate data, but there will likely be a few updates. Do not update the `Metric` column, but update all the others if you identify any discrepancies. If you notice a discrepancy in the spelling of a demographic or the format of the column for a single demographic (rather than a change in the formatting of all of them), make a note of it and move on. Any updates to the `Original Metric` column, should also be updated in the `Name Crosswalk` sheet. I'd suggest filtering to the most recent year and sorting by sheet. It may also help to create a new sheet in the report card that transposes the header for whatever sheet you're currently reviewing. Make sure when you're done, you don't save any edits you made to the report card file.

5. If you have noticed any mispellings of demographics or odd formatting errors, go to the second cell in the `Preprocessing` section of the jupyter notebook and add a new `report_card[year].columns.str.replace(___)` statement. You can copy one of the existing ones and just change the strings. For misspelled demographics, just replace the demographic, but for formatting issues, try to replace the whole column name. Beware using replace statements. Make sure to think through the consequences of replacing every instance of a string. For example, if "Asian" is misspelled "As" somewhere in a column, replacing "As" with "Asian" everywhere might result in every instance of the word "classes" being replaced to "clasianses". Regular expressions can be used to safely replace, but typically replacing an entire column name is safe enough if you are unsure of potential consequences.
   NOTE: REWRITE THIS FOR CLARITY LATER

## 3. Adding New Metrics

1. Add the name you'd like the metric to be called in the new sheet (before any demographic descriptions) as a new column header on the `Name Crosswalk` sheet in the crosswalk file. Put the actual name of the metric in the new year's row of that column. For example, the metric "High School 4-Year Cohort Graduates - Total" has "High School 4-Year Cohort Graduates" as a column header, and "High School 4-Year Cohort Graduates - Total" in the 2024 entry.

2. Extend the index row below the table exactly to the end of the table.

3. Go to the `Details` page and create a new row. The new year should go in the `Year` column, the new column header should go in the `Metric` column. The new table entry should go in the `Original Metric` column. Index should autofill. The name of the sheet of the new metric should go in `Sheet`. The `Disaggregation Format` column should be exactly a disaggregated column, with the demographic tag replaced by the word demo. The `Special Format` column can be left blank, it is there to support older report cards for a few metrics. The `Category` column should categorize the new metric as you see fit. Creating a new category requires modifying the code.

4. After a new metric is added, you should check previous report cards to ensure that it does not exist in the previous report cards. If it does exist in them, add the name of the metric to each year's row in the `Name Crosswalk` sheet and add a new row in `Details` for each year that it exists.

5. If the new metric has text info instead of number info, make sure to add it to the `text_columns` variable right before the definition of the adjust typing function.

## 4. Adding a New Category

Note: If you're not experienced in debugging, before adding a new category, I would suggest giving the metrics a temporary, existing category name (besides Identifier). Then proceed to section 5 and return here after you've finished the rest of the instructions. If you're confident in debugging though, this is a simple enough process that you can do it here and if there's an error it should be easy to spot.

1. Go to the second cell of the `Data Categorization and Writing to File` section. Add the name of the new category of the string to the list indexing cat_walk.

## 5. Rerunning the Notebook

1. If you've been editing a cloud copy of the crosswalk file (for example, Advance Illinois' `Historic Crosswalk.xlsx`), save it as a copy with the name `Local Historic Crosswalk.xlsx` to overwrite the `Local Historic Crosswalk.xlsx` in the `HistoricReportCard` folder. NOTE: If you saved as a copy, immediately close the crosswalk file. Any edits made later to the crosswalk file should be made in the cloud copy and then a new copy should be saved locally before rerunning.

2. Go to the second to last cell in the notebook and make sure the `write_to_file` variable is set to `False`. We will update it to true after debugging.

3. Hit `Run All` in the `Historic Report Card Processing.ipynb` notebook.

4. Address errors as you run into them. If an error requires you to edit the crosswalk file, ensure you are making edits in the cloud file (if applicable).

   1. If you get an `Unnamed: #` error in the demo formats cell, the index row from section 3, step 2 was extended beyond the table and needs to be shortened. Edit the `Historic Crosswalk.xlsx` file, save it as a local copy, and then close out of it before rerunning the whole notebook.

   2. If you get an error in the cell that follows the definition of `adjust_typing`, there's likely a new symbol in one of the cells. For example, in the addition of 2024, ISBE shifted from leaving obscured data blank to putting an asterisk. You can handle this by replacing it in

   3. If you get an error in the first cell of the Final Dataset Creation and Processing section, that is expected! This cell acts as a guard to ensure that you are aware which demographics are being skipped. For example, in 2024, the addition of the new "High School 4-Year Cohort Graduates" metric threw an error here with the message:

      `KeyError: "['High School 4-Year Cohort Graduates - Unknown', 'High School 4-Year Cohort Graduates - Non Binary'] not in index"`

      This error needs us to confirm that we didn't expect Non Binary or Unknown demographics for this new metric, even though it's disaggregated by other demographics. The 2024 report card does not break down 4-Year Cohort graduates this way, so we are good to go, but if you see a demographic here that you expected to see, that should be investigated. It's likely here because of a mistake in the disaggregation format in the historic crosswalk sheet, or a mispelling of a demographic that wasn't noticed earlier.

      To resolve this error, copy the acceptable ommissions and append them to the `absent_demo_combos` list in the second cell.

      NOTE: THIS PROCESS MAY CHANGE LATER

5. Go to the cell that displays the `dropped_columns` variable. If there are no dropped columns since 2019, you're good to go. If there are some dropped columns, they should only be there because that year doesn't include that information. If you can validate that that column doesn't exist, you're good to go, but if the column does exist, there's likely a misspelling in the crosswalk document or another problem somewhere. Try to track it down and resolve it. (Also would be good to glance at all the other outputs in the file to validate that there aren't any strange issues going on)
