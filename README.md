# psql2csv

**Run a query in psql and output the result as CSV.**

## Installation

### Mac OS X

psql2csv is available on [Homebrew](http://brew.sh/).

    $ brew install psql2csv

### Manual

Grab the file `psql2csv`, put in somewhere in your `$PATH`, and make it
executable:

    $ curl https://raw.githubusercontent.com/fphilipe/psql2csv/master/psql2csv > /usr/local/bin/psql2csv && chmod +x /usr/local/bin/psql2csv

## Usage

    psql2csv [OPTIONS] < QUERY
    psql2csv [OPTIONS] QUERY

## Options

The query is assumed to be the contents of STDIN, if present, or the last
argument. All other arguments are forwarded to psql except for these:

    -h, --help             show this help, then exit
    --delimiter=DELIMITER  use a different delimiter than comma (e.g. $'\t' for tab)
    --quote=QUOTE          which quoting character to use (defaults to double-quote)
    --null=NULL            specify a string that represents a NULL value (defaults to nothing)
    --encoding=ENCODING    use a different encoding than UTF8 (Excel likes LATIN1)
    --no-header            do not output a header
    --dry-run              print the query that would be run without running it

## Example Usage

    $ psql2csv dbname "select * from table" > data.csv

    $ psql2csv dbname < query.sql > data.csv

    $ psql2csv --no-header --delimiter=$'\t' --encoding=latin1 dbname <<sql
    > SELECT *
    > FROM some_table
    > WHERE some_condition
    > LIMIT 10
    > sql

## Author

Philipe Fatio ([@fphilipe](https://github.com/fphilipe))
