 *Subdomain Enumeration Project  

This project was created as part of my **Skill Horizon Internship**.  
The goal is to perform **subdomain enumeration** for target domains (`wikipedia.org` and `mozilla.org`) using multiple tools and then organize the results into a structured repository.  


📂 Project Structure

Skill_Horizon/ └── Subdomains_Enumeration/ ├── results/ │   ├── wikipedia.org/ │   │   ├── wikipedia_subfinder.txt │   │   ├── wikipedia_assetfinder.txt │   │   ├── wikipedia_all.txt │   │   └── wikipedia_alterx.txt │   ├── mozilla.org/ │   │   ├── mozilla_subfinder.txt │   │   ├── mozilla_assetfinder.txt │   │   ├── mozilla_all.txt │   │   └── mozilla_alterx.txt │   └── others/   # Any unmatched files └── README.md



🛠️ Tools Used  

 **[Subfinder](https://github.com/projectdiscovery/subfinder)** → Passive subdomain enumeration  
- **[Assetfinder](https://github.com/tomnomnom/assetfinder)** → Discover subdomains from various sources  
- **[Alterx](https://github.com/projectdiscovery/alterx)** → Generate permutations of discovered subdomains  



🚀 How It Works  

For each domain (example: `wikipedia.org`):  

1. **Subfinder** → Collects subdomains  
   ```bash
   subfinder -d wikipedia.org -silent -o wikipedia_subfinder.txt

2. Assetfinder → Collects subdomains from other sources

assetfinder --subs-only wikipedia.org > wikipedia_assetfinder.txt


3. Combine Results → Merge and remove duplicates

cat wikipedia_subfinder.txt wikipedia_assetfinder.txt | sort -u > wikipedia_all.txt


4. Alterx → Generate possible permutations

alterx wikipedia_all.txt > wikipedia_alterx.txt



Repeat the same for mozilla.org.



📊 Output Files

*_subfinder.txt → Subdomains found by Subfinder

*_assetfinder.txt → Subdomains found by Assetfinder

*_all.txt → Combined unique subdomains

*_alterx.txt → Permuted subdomains




📦 Installation

Install the tools (on Kali Linux or Debian-based systems):

# Install Subfinder
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

# Install Assetfinder
go install github.com/tomnomnom/assetfinder@latest

# Install Alterx
go install github.com/projectdiscovery/alterx/cmd/alterx@latest

Add them to your PATH:

export PATH=$PATH:$(go env GOPATH)/bin



📂 Automating with Script

To automate the workflow, create a script collect_subdomains.sh:

#!/bin/bash
domains=("wikipedia.org" "mozilla.org")

for domain in "${domains[@]}"; do
  echo "[*] Collecting subdomains for $domain"
  
  subfinder -d $domain -silent -o ${domain}_subfinder.txt
  assetfinder --subs-only $domain > ${domain}_assetfinder.txt
  
  cat ${domain}_subfinder.txt ${domain}_assetfinder.txt | sort -u > ${domain}_all.txt
  
  # Run alterx (version may vary)
  alterx ${domain}_all.txt > ${domain}_alterx.txt
done

Run it:

chmod +x collect_subdomains.sh
./collect_subdomains.sh



📝 Notes

If alterx shows “flag provided but not defined”, just run it like:

alterx domain_all.txt > domain_alterx.txt

All results are stored inside results/ for neat organization.

This project can be extended with tools like Amass, gau, waybackurls, httprobe, etc.

