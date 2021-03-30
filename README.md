# nft-geo-sets

## Description

This tool queries [RIPE NCC's Country Resource List API](https://stat.ripe.net/docs/data_api#country-resource-list) to get a list of IPv4 and IPv6 ranges of a specific country to create a corresponding set containing these ranges. nft-geo-sets does not create any other rules besides the set itself. You have to add these manually.

## Dependencies

* python3 (3.5.3+)

## /etc/nft-geo-sets.conf

Configuration elements under the DEFAULT section apply to every country as long as they are not overwritten. The section name of the country has to match the 2-digit ISO-3166 country code and is used in the query of the API.

* directory is the place where the script saves and overrides sets adhering to its naming scheme (PRIORITY-COUNTRYCODE-v4.nft and PRIORITY-COUNTRYCODE-v6.nft) of files.
* family_v4 and family_v6 are the ip families used for the ```add set [family]``` command. For more information check the nftables man page.
* table defines the table the set is added to.
* priority sets the number in front of the filename to more easily see the sequence in which nftables is loading files of a directory when using wildcards.
* ipv4 and ipv6 set, whether to download ipv4 and/or ipv6 ranges for a country.

```
[DEFAULT]
directory = /etc/nftables.d
family_v4 = ip
family_v6 = ip6
table = filter
priority = 10

[de]
ipv4 = yes
ipv6 = no

[nl]
ipv4 = yes
ipv6 = yes
```

## Usage

Creating the configured sets.
```
nft-geo-sets
```

Only checking, which files would be written (dry run).
```
nft-geo-sets -d
```

Debug including verbose output (print to stdout).
```
nft-geo-sets -l DEBUG -v
```

Change the default log directory.
```
nft-geo-sets -L /var/log/custom_log_directory
```

