# 📂 Public Cloud IP Ranges Extractor

This project aggregates public IP ranges from major cloud providers and outputs them into a unified, structured CSV file for further processing, enrichment, or integration in security systems.

## 📅 Schedule

The pipeline is run weekly via GitHub Actions (cron: `0 3 * * *`) and can also be triggered manually.

---

## 🔗 Sources

The IP range data is retrieved programmatically from the official public endpoints of each provider:

| Provider             | Source URL (format)                                                                                                                                                                      | Notes                                         |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| Google Cloud         | [https://www.gstatic.com/ipranges/cloud.json](https://www.gstatic.com/ipranges/cloud.json)                                                                                               | includes `region` and `service`               |
| AWS                  | [https://ip-ranges.amazonaws.com/ip-ranges.json](https://ip-ranges.amazonaws.com/ip-ranges.json)                                                                                         | includes both IPv4 and IPv6                   |
| Microsoft Azure      | [https://download.microsoft.com/download/7/1/d/71d86715-5596-4529-9b13-da13a5de5b63/ServiceTags_Public_20250505.json](https://download.microsoft.com/download/7/1/d/71d86715-5596-4529-9b13-da13a5de5b63/ServiceTags_Public_20250505.json)                                                                                                                                                            | Microsoft Service Tags JSON    |
| Digital Ocean        | [https://digitalocean.com/geo/google.csv](https://digitalocean.com/geo/google.csv)                                                                                                       | geolocated per region                         |
| Linode               | [https://geoip.linode.com/](https://geoip.linode.com/)                                                                                                                                   | provides IP and region                        |
| Oracle Cloud         | [https://docs.oracle.com/en-us/iaas/tools/public\_ip\_ranges.json](https://docs.oracle.com/en-us/iaas/tools/public_ip_ranges.json)                                                       | includes `tags` per prefix                    |
| IBM Cloud            | [https://cloud.ibm.com/docs/infrastructure-hub?topic=infrastructure-hub-ibm-cloud-ip-ranges](https://cloud.ibm.com/docs/infrastructure-hub?topic=infrastructure-hub-ibm-cloud-ip-ranges) | parsed from HTML tables                       |
| Cloudflare           | [https://www.cloudflare.com/ips-v4](https://www.cloudflare.com/ips-v4) / ips-v6                                                                                                          | clean text format by IP version               |
| Fastly               | [https://api.fastly.com/public-ip-list](https://api.fastly.com/public-ip-list)                                                                                                           | JSON split IPv4 / IPv6                        |
| iCloud Private Relay | [https://mask-api.icloud.com/egress-ip-ranges.csv](https://mask-api.icloud.com/egress-ip-ranges.csv)                                                                                     | CSV with geolocation                          |

---

## 📁 Output Format

The output file is `public_cloud_ip_ranges.csv` and contains the following fields:

| Column            | Description                      |
| ----------------- | -------------------------------- |
| `prefix`          | CIDR range (IPv4 or IPv6)        |
| `platform`        | Provider (e.g. AWS, GCP, Oracle) |
| `region`          | Geographic region (if available) |
| `service`         | Cloud service (if specified)     |
| `networkFeatures` | Tags or notes about the IP block |
| `ip_version`      | 4 or 6                           |

The data is normalized using `ipaddress` to distinguish between IPv4/IPv6 prefixes.

---

## 📊 Use Cases

* Threat intelligence enrichment
* IP-based geolocation or attribution
* Firewall/IDS/IPS filtering and allowlisting
* Research and visualization of cloud usage

---

## 💼 Dependencies

This project uses:

* `pandas`
* `numpy`
* `requests`
* `ipaddress`
* `html5lib` (for IBM table parsing)

Install via:

```bash
pip install -r requirements.txt
```

---

## 📄 License

MIT License. You are free to reuse or adapt the code and output.

---

## 📨 Maintainer

**Stefano De Nardis**
📧 [stefano.denardis@klonet.it](mailto:stefano.denardis@klonet.it)
🌐 [https://www.klonet.it](https://www.klonet.it)
