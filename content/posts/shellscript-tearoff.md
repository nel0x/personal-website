+++
title = "Shellscript tear-off calendar"
date = "2021-11-23T16:00:00+01:00"
tags  = ['shellscripting', 'linux', 'automation', 'tech']
+++

Yesterday I came up with the idea to have the saying on my landing page dynamically change daily.

After some tinkering (to be honest: round about 14 hours) I realized this can be achived by saving all sayings line by line into a file, in this case `sayings.txt` and exchanging the website-saying, set in the `config.toml` under the `homeSubtitle` section.

Now let's get to the exciting part, to the script which actually perfoms the task:
```
#!/usr/bin/env sh

# define files
sayingslist=./sayings.txt
sayingsconfig=./config.toml

# read new saying (randomly)
newsaying=$(shuf -n 1 $sayingslist)

# replace old with new saying
sed -i "s|homeSubtitle=\".*\"$|homeSubtitle=\"$newsaying\"|" $sayingsconfig

### Sayings-sellection alternatively (in sequence) done trough:
# read new saying (1st line)
#newsaying=$(sed -n '1p' $sayingslist)
# append new saying to end of list
#echo "$newsaying" >> $sayingslist
# remove new saying from 1st line
#sed -i "1d" $sayingslist
```

After that just executing the `tear-off.sh`-script does the work like a charm.

If we now want this to happen automatically on a daily basis, we can automate the execution for example with inotify-tools to run it every day, week or month.
