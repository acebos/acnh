## ✔️ Daily Issue Checklist Guide
How to create an automatic daily checklist out of an issue! 🍂

Contains information on what to do each day.
- Manual ticking.
- Automated closing of the previous day's issue when the next day starts.

<strong> That's cool... but how do I do it? </strong>
1. Copy and paste the following code block into a new workflow.
2. Wait.
3. Seriously... Wait. This runs at 5am GMT everyday. 
<br> You can change how frequently it runs at [crontab.guru](https://crontab.guru/) and replacing the `- cron:` value.

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