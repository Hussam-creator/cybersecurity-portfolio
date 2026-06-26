# Exposed Git Repositories

## Overview

An exposed `.git` directory can unintentionally disclose the complete version history of a web application. If accessible over HTTP, attackers may be able to reconstruct the repository and recover source code, configuration files, credentials, API keys, or sensitive information that is not publicly visible. Identifying exposed Git repositories is an important part of web application enumeration, as leaked source code can significantly improve vulnerability discovery and exploitation.

---
```
git-dumper.py http://site.com/.git/
```

```
git log
git status
git show <commit>
git cat-file commit <commit>
```
