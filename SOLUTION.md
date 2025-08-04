# Git Challenge Solution

This repository contains the solution to the [Interlincx Git Challenge](https://github.com/Interlincx/challenge-git).

## Challenge Overview

The challenge required using `git rebase` to bring commits from two feature branches (`add-echo` and `add-reverse`) onto the master branch, creating a linear history without merge commits.

## Solution Approach

### 1. Initial State Analysis
- `master` branch: Latest commit with documentation updates
- `add-echo` branch: Added echo route functionality
- `add-reverse` branch: Added reverse route functionality
- Both feature branches were based on an earlier commit than current master

### 2. Rebase Strategy
Instead of using `git merge` (which would create merge commits), we used `git rebase` to maintain a linear history:

```bash
# 1. Rebase add-echo onto master
git checkout add-echo
git rebase master

# 2. Rebase add-reverse onto the rebased add-echo
git checkout add-reverse  
git rebase add-echo

# 3. Move master to point to the final commit
git checkout master
git reset --hard add-reverse
```

### 3. Conflict Resolution
During the rebase process, merge conflicts occurred in:
- `lib/api.js` - Both branches added function exports
- `lib/server.js` - Both branches added route definitions  
- `test/routes.js` - Both branches added test cases

These were resolved by combining both sets of changes rather than choosing one over the other.

## Final Result

The solution achieved a clean, linear git history:

```
ede5487 feat: add reverse route
ef9edbc feat: add echo route  
2bd7712 docs: add more instructions
08d29e7 chore: update deps
744a0f1 docs: update readme for git challenge
...
```

## Key Features Added

### Echo Route
- **Endpoint**: `GET /echo?param1=value1&param2=value2`
- **Functionality**: Returns query parameters as JSON
- **Implementation**: `api.echo()` function

### Reverse Route  
- **Endpoint**: `GET /reverse/:input`
- **Functionality**: Reverses the input string
- **Implementation**: `api.reverse()` function
- **Example**: `/reverse/hello` returns `{"input": "hello", "output": "olleh"}`

## Testing

Both routes include comprehensive test coverage in `test/routes.js`:
- Echo route test validates query parameter echoing
- Reverse route test validates string reversal functionality

## Repository Setup

This solution was pushed to: https://github.com/codyrutscher/git-challenge-solution.git

The original challenge repository: https://github.com/Interlincx/challenge-git