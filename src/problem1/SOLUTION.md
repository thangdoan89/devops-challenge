Provide your CLI command here:

grep "TSLA" transaction-log.txt | jq -r '.order_id' | while read order_id; do curl "https://example.com/api/$order_id" >> output.txt; done