## ✔️ Daily Issue Checklist Guide
**Table of Contents**
- [v3 Template](#v3-template) 
- [v2 Template](#v2-template) 
- [How to Guide](#that'scool-but-how-do-i-do-it?) -That's cool but how do I do it?

How to create an automatic daily checklist out of an issue! 🍂
<br>Contains information on what to do each day.
- Manual ticking.
- Automated closing of the previous day's issue when the next day starts.

#### <strong>That's cool but how do I do it? </strong>
1. Copy and paste the following code block (v2 or v3 it's up to you) into a blank workflow.
2. Wait.
3. Seriously... Wait. This runs at 5am GMT everyday. 
<br> - You can change how frequently it runs at [crontab.guru](https://crontab.guru/) and replacing the `- cron:` value.
<br> - If you use the v3 template you can manually run the workflow from the actions tab.

### v3 Template

``` YAML
name: Daily Checklist v3
on:
  workflow_dispatch:
  schedule:
  # Updates at 5am everyday (when a new day starts in ACNH) – https://crontab.guru 
  - cron: 0 5 * * *

jobs:
  daily_standup:
    name: Daily Tasklist Issue
    runs-on: ubuntu-latest
    steps:

    - name: Today's date
      run: echo "TODAY=$(date '+%Y-%m-%d')" >> $GITHUB_ENV

    # Generates and pins new standup issue, closes previous, writes linking comments, and assigns to all assignees in list
    - name: Daily Checklist 🕔
      uses: imjohnbo/issue-bot@v3
      with:
        assignees: "fluteds" # list the github handles without the @ e.g. "fluteds, user2"
        labels: "daily checklist" # assigns a ready made label to the issue
        title: Daily ACNH Checklist 🕔 # title of the issue
        body: |- # edit the your body text below, this is the description of the issue
          ## 🌲 ACNH Checklist for ${{ env.TODAY }}
          :wave: Hi, {{#each assignees}}@{{this}}{{#unless @last}}, {{/unless}}{{/each}}! Here's your daily checklist.
          Resets at 🕔 **`5:00 am`** local time.
          * [ ] Check in at the Nook Stop (300 Nook Miles per day after your first 6 days)
          * [ ] Shop the Nook Stop for new special catalog items and fence recipes
          * [ ] Shop at Nook’s Cranny and the Able Sisters’
          * [ ] Complete the first 5 Nook Miles+ missions of the day for 2x bonus
          * [ ] Find your island’s 4 fossils
          * [ ] Find your island’s DIY Recipe message in a bottle
          * [ ] Find your island’s glowing money spot for 💰1,000 Bells. (Optional: Re-plant money in the spot to grow a “money tree" to multiply your investment!)
          * [ ] Harvest resources
            * [ ] Rocks (materials — including up to 💰16,000 Bells from one rock)
            * [ ] Trees
              * [ ] Wood
              * [ ] Fruits (grow every three days after they were last picked)
              * [ ] Furniture, ~💰1,000 Bells, and/or wasps from cedar/hardwood trees
        pinned: false # whether to pin the issue
        close-previous: true # whether is close the previous issue 
        linked-comments: false # whether to add a comment with #number of last issue
```

### v2 Template

``` YAML
name: Daily Tasklist Issue # name of workflow
on:
  schedule:
  - cron: "0 5 * * *"  # creates a new issue and closes the previous issue open at 5am everyday (when a new day starts in ACNH) – https://crontab.guru 

jobs:
  daily_standup:
    name: Daily Tasklist Issue
    runs-on: ubuntu-latest
    steps:

    # keep the following two lines if you fill in a value for `template` 
    - name: Checkout
      uses: actions/checkout@v2

    - name: issue-bot
      uses: imjohnbo/issue-bot@v2
      with:
        assignees: "fluteds" # github handles without the @ eg: "user1, user2"
        labels: "daily checklist" # write the tag name separated by commas if you have more than one eg: "daily checklist, tagname"
        pinned: true # whether to pin the issue to the top
        close-previous: true # if set to true, the previous issue will be closed 
        template: ".github/ISSUE_TEMPLATE/daily-checklist.md" # the daily checklist template location. this can be swapped out for any template you like in this location
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} 
```
