# dpmv

> DNS Provider Migration Verification

## Why I Built This

While I was working at Adobe we needed to switch our DNS provider from [Dyn] to
[R53]. [R53] does not support [AXFR], so ensuring our DNS database was migrated
successfully was of utmost importance. Fortunately, [Dyn] provided [zone files]
to import into [R53], but full reliance on the import was not practical in our
situation. This script is meant to provide a mechanism for verifying your DNS
database is as exact as possible.

## How It Works

Given a [zone file] in [BIND] format provided from your current DNS provider, the primary
name server of your current DNS provider, and the primary name server of the DNS
provider you are migrating to, the script iterates the [zone file] line by
line, querying the records at both name servers, and diffing their contents.
Name servers and SOA records are ignored at the top-level.

## Usage

Download the script, make it executable, and run it.

```bash
$ dpmv

USAGE: dpmv zonefile.txt nameserverA nameserverB
```

## Caveats

Zone files are hard to parse. This script does not support multi-line resource
records in the zone file. This is typically only with the SOA record anyway.

```
@                      3600 SOA   ns1.p23.dynect.net. (
                              hostmaster.typekit.com.    ; address of responsible party
                              202                        ; serial number
                              3600                       ; refresh period
                              600                        ; retry period
                              604800                     ; expire time
                              60                       ) ; minimum ttl
```

[Dyn]: https://dyn.com
[R53]: https://aws.amazon.com/route53/
[AXFR]: https://en.wikipedia.org/wiki/DNS_zone_transfer
[zone file]: https://en.wikipedia.org/wiki/Zone_file
[zone files]: https://en.wikipedia.org/wiki/Zone_file
[BIND]: https://en.wikipedia.org/wiki/BIND
