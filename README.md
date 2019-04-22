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

## Advanced Usage

Let's assume you have a script `monthly_report.sql` that you run every month.
This script has a `WHERE` that limits the report to a certain month:

    WHERE date_trunc('month', created_at) = '2019-01-01'

Every time you run it you have to edit the script to change the month you want
to run it for. Wouldn't it be nice to be able to specify a variable instead?

Turns out psql does have support for [variables] which you can pass to psql (and
thus to psql2csv) via `-v`, `--variable`, or `--set`. To interpolate the
variable into the query you can use `:VAR` for the literal value, `:'VAR'` for
the value as a string, or `:"VAR"` for the value as an identifier.

Let's change the `WHERE` clause in `monthly_report.sql` file to use a variable
instead:

    WHERE date_trunc('month', created_at) = (:MONTH || '-01')::timestamptz

With this change we can now run the query for any desired month as follows:

    $ psql2csv -v MONTH=2019-02 < monthly_report.sql > data.csv

[variables]: https://www.postgresql.org/docs/current/app-psql.html#APP-PSQL-VARIABLES

## Further Help

- [PostgreSQL COPY documentation](http://www.postgresql.org/docs/current/static/sql-copy.html)
- [psql variables](https://www.postgresql.org/docs/current/app-psql.html#APP-PSQL-VARIABLES)

## Author

Philipe Fatio ([@fphilipe](https://github.com/fphilipe))
