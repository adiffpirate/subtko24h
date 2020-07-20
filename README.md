# SubTko24h

A 24/7 subdomain takeovers finder that glues together some open source tools using bash.

> Subdomain takeover is a type of vulnerability where the attacker gains total control of a subdomain. It can be used to chain other vulns (like XSS), for mass phishing campaigns (that's almost impossible for the user to detect), and some other things. You can read more about it [here](https://www.hackerone.com/blog/Guide-Subdomain-Takeovers).

## Content

This script is running in cloud using a basic droplet from [digital ocean](https://www.digitalocean.com).

> It costs me only $5 per month, checking a few tens subdomains per second (it can do more, but I don't think it's necessary since the same subdomain is checked about 3 times a day).

### [Adrecon](https://github.com/adiffpirate/hacktools/blob/master/src/adrecon)

Does subdomain enumeration. Currently using:

- Passive search:
	- [Amass](https://github.com/OWASP/Amass)
	- [Subfinder](https://github.com/projectdiscovery/subfinder)
	- [Github-Subdomains.py](https://github.com/gwen001/github-search/blob/master/github-subdomains.py)
- Bruteforcing:
	- [Massdns](https://github.com/blechschmidt/massdns)
	- [Dnsgen](https://github.com/ProjectAnte/dnsgen)

### [Subtko](https://github.com/adiffpirate/hacktools/blob/master/src/subtko)

Checks for possible subdomain takeovers using:

- [Nuclei](https://github.com/projectdiscovery/nuclei)

### [Aliases](https://github.com/adiffpirate/hacktools/blob/master/.aliases)

To keep things simple.

> Doing that, I can run my tools in a much effective way by avoid typing the full command (e.g. `cat subs-list | massdns -q -r $resolvers -s 1000 -o S | awk '{print $1}' | sed 's/\.$//g' | sort -u` turns into `cat subs-list | massdns`).

### Database

To organize and making it easy to acess/read the data, a mysql database (named "recon") is used with the following tables:

#### targets

> (program VARCHAR(255) NOT NULL, domain VARCHAR(255) PRIMARY KEY)

Domains to perform subdomain enumeration.

#### data 

> (domain VARCHAR(255), subdomain VARCHAR(255) PRIMARY KEY, alive BOOLEAN DEFAULT 0, source VARCHAR(255) NOT NULL, modified TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, created TIMESTAMP DEFAULT CURRENT_TIMESTAMP)

Subdomains that will be tested for takeover.

#### takeovers 

> (url VARCHAR(255) PRIMARY KEY, service VARCHAR(255) NOT NULL, modified TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, created TIMESTAMP DEFAULT CURRENT_TIMESTAMP)

Possible takeovers found.

## Usage

What I do it's running `adrecon24h [quick|medium|full]` alongside `subtko24h` to continuous enumerate subdomains and test them.
_(A friendly tip here, you can use [screen](https://linuxize.com/post/how-to-use-linux-screen/) to run more than one script at the same time with only one terminal)_

> If you don't want to use adrecon, you can enumerate subdomains in your way and add them to db with <br>`cat {yourSubdomainsList} | addtodb subs`.<br>But be aware that your list MUST have the following format each line: `subdomain source [alive|dead]`
