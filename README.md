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

    -?, --help                 show this help, then exit
    --delimiter=DELIMITER      set the field delimiter (default: ,)
    --quote=QUOTE              set the quote delimiter (default: ")
    --escape=ESCAPE            set the escape character (default: QUOTE)
    --null=NULL                set the string representing NULL; printed without quotes (default: empty string)
    --force-quote=FORCE_QUOTE  set the columns to be force quoted; comma separated list of columns or * for all (default: none)
    --encoding=ENCODING        set the output encoding; Excel likes latin1 (default: UTF8)
    --no-header                do not output a header
    --timezone=TIMEZONE        set the timezone config before running the query
    --search-path=SEARCH_PATH  set the search_path config before running the query
    --dry-run                  print the COPY statement that would be run without actually running it

## Example Usage

    $ psql2csv dbname "select * from table" > data.csv

    $ psql2csv dbname < query.sql > data.csv

    $ psql2csv --no-header --delimiter=$'\t' --encoding=latin1 dbname <<sql
    > SELECT *
    > FROM some_table
    > WHERE some_condition
    > LIMIT 10
    > sql

## Further Help

- [PostgreSQL COPY documentation](http://www.postgresql.org/docs/current/static/sql-copy.html)

## Author

Philipe Fatio ([@fphilipe](https://github.com/fphilipe))
