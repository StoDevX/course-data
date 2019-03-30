St. Olaf Course Data
====================

This repository holds the data for Gobbldygook in both raw (XML) and proccessed (JSON) forms. You do not need this repository if your are only interested in using Gobbldygook.


## Usage

If you are interested in using the course data in another tool, it's hosted through Github on the [`gh-pages` branch](https://github.com/stodevx/course-data/tree/gh-pages).

You can download it however you'd like, such as via `curl`. Assuming you want the 2016-Fall data in `csv` format (aka the file `20161.csv`), you'd do:

```console
curl https://stolaf.dev/course-data/terms/20161.json
```

We have data from 1994-Fall through this year, whetever this year is, automatically updated from the SIS's [Class & Lab page](https://www.stolaf.edu/sis/public-aclasslab.cfm) once a day.


## Changes

We make some changes to the original XML data we get from the SIS, as detailed in the table below.

from `key` | to `key` | change details
----|-----|---
`clbid` | | String to int
`coursename` | `name` | Unescape HTML entities
`coursenumber` | `level` | Extract the "level" (number from 1-3) from the course number
`coursenumber` | `number` | String to int
`coursesection` | `section` | None
`coursestatus` | `status` | None
`coursesubtype` | `type` | Expand the letter to a word; e.g., `R` to `Research`
`credits` | | String to float
`crsid` | | String to int
`deptname` | `departments` | Put the departments into a list
`description` |  | If the course description (in the SIS detail window) is different from the `notes`, it's embedded here
`gereqs` | | Extract text from the HTML provided, and turn them into a list
`groupid` | | String to int
`instructors` | | Extract text from the HTML provided, turn them into a list, and flip them to be "First Last", instead of "Last, First"
`meetinglocations` | `locations` | Extract text from the HTML provided, and turn them into a list
`meetingtimes` | `times` | Extract text from the HTML provided, and turn them into a list
`notes` | | Remove `<br>` tags and extraneous whitespace
`pn` | | `Y/N` to boolean
`prerequisites` |  | Simple implementation: look for the string `Prereq` in `notes` and `description`, then taken everything from "Prereq" to the end of the string and put it in `prerequisites`
`revisions` |  | A recorded history of revisions of the course
`term` | `term`, `year`, `semester` | Take the five-digit term string, turn it into a number, and split it into `year` and `semester` for simplicity
`title` |  | If the course title (in the SIS detail window) is different from the `name`, it's embedded here
`varcredits` | | Remove it entirely. I have no idea what it does; it's set to `N` for every single course in the database.


## Revisions
A `revision` records what changed in a course. Every revision will have an `_updated` property, which records when the revision was recorded<a href="#fn-1">¹</a>, as well as what changed. The other key/value pairs in the revision are the old values for those keys; `null` indicates that there was no old value.

1: <a name="fn-1"/> Note that this is when it was _recorded_, not necessarily when it _happened_. Sometimes the updater breaks, so a bunch of changes get grouped into one update.
