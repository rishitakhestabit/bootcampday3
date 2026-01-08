# Git Merge Conflict & Two-Clone Workflow – Postmortem

## Objective

The goal of this task was to simulate a real-world Git collaboration scenario by:

- Using two clones of the same repository
- Editing the same line in the same file
- Creating a merge conflict
- Resolving the conflict while keeping both changes
- Ensuring the commit graph shows branches and a merge commit


## Pushing the Base Repository to GitHub

I started by going to my main working repository:

```bash
cd ~/bootcampday3
git status
```
At this point:
 My branch was up to date with origin/master
 I had some untracked files such as:

 bisect-session.txt
 screenshots/
I then pushed the repository to GitHub:
git push origin master
This ensured that the bootcampday3 repository existed on GitHub and could be cloned.

Then I created Two Clones (Repo-A and Repo-B)

To simulate two developers working independently, I cloned the same GitHub repository into two different directories:

cd ~
git clone https://github.com/rishitakhestabit/bootcampday3.git repo-A
git clone https://github.com/rishitakhestabit/bootcampday3.git repo-B

Now:

    repo-A represents Developer A
    repo-B represents Developer B

Both repositories started from the exact same commit history.

I made a Change in Repo-A

I navigated into repo-A and edited the calculator.js file.

cd ~/repo-A
nano calculator.js

I modified the log related to the div function (same line that would later be edited in Repo-B).

Then I committed and pushed the change:

git add calculator.js
git commit -m "Repo-A: modify div log message"
git push origin master

Making a Conflicting Change in Repo-B

Next, I moved to repo-B and edited the same file and same line:

cd ~/repo-B
nano calculator.js

Before pulling the latest changes, Repo-B had local modifications.

When I tried to pull:

git pull --no-rebase origin master

Git correctly stopped me with this error:

    Your local changes would be overwritten by merge.

Committing Local Changes in Repo-B
To proceed safely, I committed my local change in Repo-B:
git add calculator.js
git commit -m "added line b"
Now Repo-B had its own commit, while GitHub had Repo-A’s commit.

Pulling and Creating a Merge Conflict
After committing, I pulled again:
git pull --no-rebase
This time Git attempted a merge and produced a merge conflict:
CONFLICT (content): Merge conflict in calculator.js
Automatic merge failed

This happened because:

    Repo-A and Repo-B modified the same line
    Git could not decide automatically which change to keep

![Merge Conflict](screenshots/mergeconflict.png)

#Resolving the Merge Conflict (Keeping Both Changes)

I opened the conflicted file:

nano calculator.js

Inside the conflict markers, I manually edited the file to keep both changes, ensuring no logic was lost.
After resolving the conflict, I staged and committed the merge:
git add calculator.js
git commit -m "Merge conflict resolved"
This created a true merge commit.

![Conflict markers before resolution](screenshots/markersmergeconflict.png)

![Final resolved file](screenshots/correctmarkers.png)




Step 8: Verifying the Commit Graph

To confirm that the task requirement was met, I checked the commit graph:

git log --oneline --graph --all


*   6ede0d3 Merge conflict resolved
|\  
| * a9ce0f3 Repo-A: modify div log message
* | f33ebfc added line b
|/  
* cfbbadf previous commits...

This shows:

    Two parallel commits

    A merge commit joining them

    git log --graph --all output

![Final resolved file](screenshots/graph.png)


#Pushing the Merge Commit

Finally, I pushed the resolved merge back to GitHub:

git push origin master


