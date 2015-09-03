# psql2csv

**Run a query in psql and output the result as CSV.**

## Usage

    psql2csv [OPTIONS] < QUERY
    psql2csv [OPTIONS] QUERY

## Options

The query is assumed to be the contents of STDIN, if present, or the last
argument. All other arguments are forwarded to psql except for these:

    -h, --help           show this help, then exit
    --encoding=ENCODING  use a different encoding than UTF8 (Excel likes LATIN1)
    --no-header          do not output a header

## Example Usage

    $ psql2csv dbname --query "select * from table" > data.csv

    $ < query.sql psql2csv dbname > data.csv

    $ cat <<sql | psql2csv --no-header --encoding=latin1 dbname
    > SELECT *
    > FROM some_table
    > WHERE some_condition
    > LIMIT 10
    > sql

## Author

Philipe Fatio ([@fphilipe](https://github.com/fphilipe))
