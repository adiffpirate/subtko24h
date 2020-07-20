# SubTko24h

A 24/7 subdomain takeovers finder.

> Subdomain takeover is a type of vulnerability where the attacker has total control of a subdomain. It can be used to chain other vulns (like XSS), for mass phishing campaigns (that's almost impossible for the user to detect), and some other things. You can read more about it [here](https://www.hackerone.com/blog/Guide-Subdomain-Takeovers).

## Content

This script is running in cloud using a basic droplet from [digital ocean](https://www.digitalocean.com).

> It costs me only $5 per month, checking a few tens subdomains per second (it can do more, but I don't think it's necessary since the same subdomain is checked about 3 times a day).

### [Adrecon](https://github.com/adiffpirate/hacktools/blob/master/bash/adrecon)

Does subdomain enumeration. Currently using:

- Passive search:
	- [Amass](https://github.com/OWASP/Amass)
	- [Subfinder](https://github.com/projectdiscovery/subfinder)
	- [Github-Subdomains.py](https://github.com/gwen001/github-search/blob/master/github-subdomains.py)
- Bruteforcing:
	- [Massdns](https://github.com/blechschmidt/massdns)
	- [Dnsgen](https://github.com/ProjectAnte/dnsgen)

### [Subtko](https://github.com/adiffpirate/hacktools/blob/master/bash/subtko)

Checks for possible subdomain takeovers using:

- [Nuclei](https://github.com/projectdiscovery/nuclei)

### [Aliases](https://github.com/adiffpirate/hacktools/blob/master/.aliases)

To keep things simple.

> Doing that, I can run my tools in a much effective way by avoid typing the full command (e.g. `cat subs-list | massdns -q -r $resolvers -s 1000 -o S | awk '{print $1}' | sed 's/\.$//g' | sort -u` turns into `cat subs-list | massdns`).

### Database

To organize and making it easy to acess/read the data, a mysql database (named "recon") is used with the following tables:

- targets (program VARCHAR(255) NOT NULL, domain VARCHAR(255) PRIMARY KEY)
- data (domain VARCHAR(255), subdomain VARCHAR(255) PRIMARY KEY, alive BOOLEAN DEFAULT 0, source VARCHAR(255) NOT NULL, modified TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, created TIMESTAMP DEFAULT CURRENT_TIMESTAMP)
- takeovers (url VARCHAR(255) PRIMARY KEY, service VARCHAR(255) NOT NULL, modified TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, created TIMESTAMP DEFAULT CURRENT_TIMESTAMP)
