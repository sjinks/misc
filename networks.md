## [Censys](https://rdap.arin.net/registry/entity/CENSY)

* 2602:80D:1000::/44
* 2620:96:E000::/48
* 162.142.125.0/24
* 167.248.133.0/24
* 167.94.138.0/24
* 167.94.145.0/24
* 167.94.146.0/24
* 198.108.204.216/29
* 199.45.154.0/23
* 206.168.32.0/22
* 66.132.148.0/24
* 66.132.153.0/24
* 66.132.159.0/24
* 66.132.172.0/24
* 66.132.175.0/24
* 66.132.180.0/24
* 66.132.186.0/24
* 66.132.195.0/24
* 66.132.198.0/24
* 66.132.209.0/24
* 66.132.224.0/24
* 66.132.228.0/24
* 66.132.233.0/24
* 66.132.237.0/24
* 66.132.245.0/24
* 66.132.255.0/24
* 66.234.1.0/24
* 74.120.14.0/24

See also: https://docs.censys.com/docs/opt-out-of-data-collection

## [Palo Alto Networks](https://rdap.arin.net/registry/entity/PAN-22)

* 2604:A940::/32
* 2606:F4C0::/32
* 128.77.0.0/17
* 130.41.0.0/16
* 134.231.128.0/17
* 134.238.0.0/16
* 136.227.140.0/22
* 137.83.192.0/18
* 139.180.240.0/20
* 140.209.192.0/18
* 144.125.0.0/16
* 147.185.132.0/22
* 147.185.136.0/22
* 153.72.0.0/16
* 162.247.12.0/22
* 162.247.12.0/23
* 165.1.128.0/17
* 165.85.0.0/16
* 167.94.198.0/24
* 168.149.240.0/21
* 169.224.128.0/18
* 198.135.184.0/24
* 198.235.24.0/24
* 202.181.128.0/21
* 204.87.186.0/24
* 205.169.39.0/24
* 205.210.31.0/24
* 208.127.0.0/16
* 216.25.88.0/21
* 66.159.192.0/19
* 66.179.104.0/24
* 66.179.148.0/23
* 66.179.148.0/24
* 66.179.149.0/24
* 66.232.32.0/20
* 69.84.192.0/20
* 74.221.128.0/20
* 96.9.96.0/23

## Tiny Helper

`handle` is the organization handle.

* ARIN: `https://rdap.arin.net/registry/entity/${handle}`
* RIPE: `https://rdap.db.ripe.net/entity/${handle}`

```js
const response = await fetch(`https://rdap.arin.net/registry/entity/${handle}`);
const json = await response.json();
for (const network of json.networks) {
    for (const cidr of network.cidr0_cidrs) {
        if (cidr.v4prefix) {
            console.log('*', `${cidr.v4prefix}/${cidr.length}`);
        } else {
            console.log('*', `${cidr.v6prefix}/${cidr.length}`);
        }
    }
}
```

## See Also

* https://github.com/Connie-Wild/scanner-ip-list

* 
