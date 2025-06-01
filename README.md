# collect-urls.sh
this scripts 4th tools modyfy katana,hakrawler,waybackurls,gau

useages: bash collect-urls.sh example.com
or bash collect-urls.sh exmpale.txt


#!/bin/bash

# -------- Configuration --------
domain_input=$1
output_dir="output"
mkdir -p "$output_dir"

# -------- Function: Process single domain --------
collect_urls() {
    domain=$1
    echo "[*] Collecting URLs for: $domain"

    # katana
    echo "[*] Running katana..."
    katana -u "$domain" -silent >> "$output_dir/katana_$domain.txt"

    # hakrawler
    echo "[*] Running hakrawler..."
    echo "$domain" | hakrawler -subs -depth 2 >> "$output_dir/hakrawler_$domain.txt"

    # gau
    echo "[*] Running gau..."
    echo "$domain" | gau >> "$output_dir/gau_$domain.txt"

    # waybackurls
    echo "[*] Running waybackurls..."
    echo "$domain" | waybackurls >> "$output_dir/waybackurls_$domain.txt"
}

# -------- Input check --------
if [[ -f "$domain_input" ]]; then
    echo "[*] File detected. Running for all domains in $domain_input"
    while read -r line; do
        collect_urls "$line"
    done < "$domain_input"
elif [[ "$domain_input" =~ ^[a-zA-Z0-9.-]+$ ]]; then
    echo "[*] Single domain detected."
    collect_urls "$domain_input"
else
    echo "Usage: $0 <domain.com> or <subdomain.txt>"
    exit 1
fi

# -------- Merge and Cleanup --------
echo "[*] Merging and cleaning..."
cat "$output_dir"/*.txt | sort -u > "$output_dir/final_urls.txt"
echo "[*] Total Unique URLs Collected: $(wc -l < "$output_dir/final_urls.txt")"
echo "[*] Output saved in: $output_dir/final_urls.txt"
