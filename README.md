#  CSVInputFormat

Adapted from https://github.com/mvallebr/CSVInputFormat.

Protect against edge cases when processing splits that mess with the quote counting behaviour.

Config:

FORMAT_DELIMITER - quote character (")
FORMAT_SEPARATOR - column delimiter (,)
IS_ZIPFILE - (true)
VALID_LINE_START_PATTERN - to determine if the split has been cut at the start of a valid record (e.g. "-?\d+,")
EXPECTED_COLUMN_COUNT - expected number of columns in the record (to help determine above, e.g. 11)

Input format for hadoop able to read multiline CSVs

Run BasicTest.java to see it working. Check src/test/resource/test.csv to see a multiline demofile.

The key returned is the file position where the line starts and the value is a List with the column values

Zip files are supported.

## Example

If we read this CSV (note that line 2 is multiline):

	Joe Demo,"2 Demo Street,
	Demoville,
	Australia. 2615",joe@someaddress.com
	Jim Sample,"3 Sample Street, Sampleville, Australia. 2615",jim@sample.com
	Jack Example,"1 Example Street, Exampleville, Australia.
	2615",jack@example.com


The output is as follows:

	==> TestMapper
	==> key=0
	==> val[0] = Joe Demo
	==> val[1] = 2 Demo Street, 
	Demoville, 
	Australia. 261
	==> val[2] = joe@someaddress.com
	
	==> TestMapper
	==> key=73
	==> val[0] = Jim Sample
	==> val[1] = 
	==> val[2] = jim@sample.com

	==> TestMapper
	==> key=10
	==> val[0] = Jack Example
	==> val[1] = 1 Example Street, Exampleville, Australia. 261
	==> val[2] = jack@example.com