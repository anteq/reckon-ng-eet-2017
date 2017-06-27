# Recon-ng
a web reconnaisance framework

---

# Why?

Before you start an active attack it's really useful to **gather as much information** on your target as possible.

However, collecting all the information and keeping track of all data (e-mails, logins, accounts, names...) is rather a **repetitive and tedious task**.

That's why we have...
### web reconnaisance frameworks

They help you gather information from thirdparty sources...<br /><br />
![Gather](img/world.png)

... keep track of the data that you already have ...<br /><br />
![Track](img/sort.png)

... and discover useful connections between your entries.<br /><br />
![Discover](img/connect.png)

---

# Recon-ng

### Recon-ng
- written in Python 3
- easy to understand
- easy to use
- easy to extend
- usage is similar to Metasploit
- allows to interact with data in SQL style

### Author
Tim Tomes AKA LaNMaSteR53
https://bitbucket.org/LaNMaSteR53/recon-ng
![Tim Tomes](img/tim_tomes.jpg)

#### From the author
*Recon-ng has a look and feel similar to the Metasploit Framework,
reducing the learning curve for leveraging the framework.
However, it is quite different. **Recon-ng is not intended 
to compete with existing frameworks, as it is designed
exclusively for web-based open source reconnaissance.** *

#### From the author<br />
*If you want to exploit...<br />
... use the Metasploit Framework.<br /><br />
If you want to social engineer<br />
... use the Social-Engineer Toolkit.<br /><br />
If you want to conduct reconnaissance<br />
... use **Recon-ng**!*

---

# Let's try it!

### Hello
```
> git clone https://bitbucket.org/LaNMaSteR53/recon-ng
> cd recon-ng
> pip install -r REQUIREMENTS
> python recon-ng
```

### Hello         
```
    _/_/_/    _/_/_/_/    _/_/_/    _/_/_/    _/      _/            _/      _/    _/_/_/
   _/    _/  _/        _/        _/      _/  _/_/    _/            _/_/    _/  _/       
  _/_/_/    _/_/_/    _/        _/      _/  _/  _/  _/  _/_/_/_/  _/  _/  _/  _/  _/_/_/
 _/    _/  _/        _/        _/      _/  _/    _/_/            _/    _/_/  _/      _/ 
_/    _/  _/_/_/_/    _/_/_/    _/_/_/    _/      _/            _/      _/    _/_/_/    
 
 
  
[77] Recon modules
[8]  Reporting modules
[2]  Import modules
[2]  Exploitation modules
[2]  Discovery modules
[1]  Disabled modules
[recon-ng][default] > 
```

### Different module types
- `recon` - web reconnaisance modules
- `reporting` - export data to csv, pdf...
- `import` - import data from csv, ...

```
[recon-ng][default] > search github
[*] Searching for 'github'...
  Recon
  -----
    recon/companies-multi/github_miner
    recon/profiles-contacts/github_users
    recon/profiles-repositories/github_repos
    recon/repositories-profiles/github_commits
    recon/repositories-vulnerabilities/github_dorks
```
**Search** for module you need, using keywords.

### Naming convention<br />
`recon/${FROM}-${TO}/${SERVICE}`<br /><br />
Such module tries to map the data from `FROM` to `TO` using `SERVICE`.

### Types of data
`FROM` and `TO` can be: 
- `companies`
- `profiles`
- `domains`
- `hosts`
- `locations`
- `...`

### See the schema
For full list of what-is-what and what-is-where, check out
```
[recon-ng][default] > show schema
```

### See the schema
For example:
```
  +--------------------+    +--------------------+
  |      contacts      |    |    repositories    |
  +--------------------+    +--------------------+
  | first_name  | TEXT |    | name        | TEXT |
  | middle_name | TEXT |    | owner       | TEXT |
  | last_name   | TEXT |    | description | TEXT |
  | email       | TEXT |    | resource    | TEXT |
  | title       | TEXT |    | category    | TEXT |
  | region      | TEXT |    | url         | TEXT |
  | country     | TEXT |    | module      | TEXT |
  | module      | TEXT |    +--------------------+
  +--------------------+
```

```
[recon-ng][default] > load recon/profiles-repositories/github_repos
[recon-ng][default][github_repos] > 
```
**Loading** a module changes the context that you're in.

``` 
[recon-ng][default][github_users] > show info
 
      Name: Github Profile Harvester
      Path: modules/recon/profiles-contacts/github_users.py
    Author: Tim Tomes (@LaNMaSteR53)
      Keys: github_api
 
Description:
  Uses the Github API to gather user info from harvested profiles. Updates the 'contacts' table with
  the results.
 
Options:
  Name    Current Value  Required  Description
  ------  -------------  --------  -----------
  SOURCE  default        yes       source of input (see 'show info' for details)
 
Source Options:
  default        SELECT DISTINCT username FROM profiles WHERE username IS NOT NULL AND resource LIKE 'Github'
  <string>       string representing a single input
  <path>         path to a file containing a list of inputs
  query <sql>    database query returning one column of inputs
```
**Show** the information about the module.

### Multiple input types
You can perform the reconnaisance for one or few selected inputs (`<string>`, `<path>`) or let recon-ng perform it for the whole database.

```
[recon-ng][default][github_users] > set SOURCE defunkt
SOURCE => defunkt
```
Set sample source...

```
[recon-ng][default][github_users] > run
[!] Message from Github: Bad credentials.
```
... and run. **Bad credentials?!**

### Acquiring API Keys
Most of thirdparty modules require an API key to work. These can be acquired in many different ways - some easier and some more difficult. Some tips from the creators are mentioned [here](https://bitbucket.org/LaNMaSteR53/recon-ng/wiki/Usage%20Guide#!acquiring-api-keys).

```
[recon-ng][default][github_users] > keys add github_api XXX_YOUR_API_KEY_XXX
[*] Key 'github_api' added.
```
Ah, that's better.

```
[recon-ng][default][github_users] > run
[*] [contact] Chris Wanstrath (chris@github.com) - Github Contributor at @github 
 
-------
SUMMARY
-------
[*] 1 total (1 new) contacts found.
[recon-ng][default][github_users] > 
```
Information about the github profile have been added.

```
[recon-ng][default][github_users] > show contacts
  +--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | rowid | first_name | middle_name | last_name |          email          |                                        title                                         |     region    | country |    module    |
  +--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 1     | Chris      |             | Wanstrath | chris@github.com        | Github Contributor at @github                                                        | San Francisco |         | github_users |
  +--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```
You can also **show** the contents of the database directly.

---

# Architecture

```python
code code sample code
```

---

# Our work

Goldenline

Facebook

NK

---

# Who are we

Teleinformatyka AGH

EET
