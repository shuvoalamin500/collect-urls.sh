script name: shuvo-crt.sh
install: permission chmod +x  shuvo-crt.sh
runs: ./ shuvo-crt.sh org name.example




#!/bin/bash

# ─────────────────────────────
#    🔍 Shuvo's crt.sh Tool 🔍
# ─────────────────────────────

clear
echo "+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+"
echo "|     ..| search crt.sh v1.1 by Shuvo |.. |"
echo "+  site : crt.sh Certificate Search       +"
echo "|       Twitter: @ShuvoRecon              |"
echo "+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+"
echo

# Usage Instruction
if [ -z "$1" ]; then
  echo "Usage: $0 \"Organization Name\""
  echo "Example: $0 \"Google LLC\""
  exit 1
fi

# Input org name
ORG_NAME="$1"
ENCODED_ORG=$(echo "$ORG_NAME" | sed 's/ /%20/g')
OUTPUT_FILE="cert_urls.txt"

echo "[*] Searching crt.sh for organization: $ORG_NAME"
curl -s "https://crt.sh/?O=$ENCODED_ORG" |
grep -oP '(?<=<TD>)[a-zA-Z0-9*.-]+\.[a-zA-Z]{2,}(?=</TD>)' |
grep -vE '^(\|\s)$' |
sort -u > "$OUTPUT_FILE"

echo
echo "[+] Found $(wc -l < $OUTPUT_FILE) unique subdomains!"
echo "[+] Saved to $OUTPUT_FILE"
