Provide your CLI command here:

while read p; do echo "$p"; done < transaction-log.txt | grep "TSLA" | curl "https://example.com/api/$(jq .order_id)"