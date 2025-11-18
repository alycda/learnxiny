---
category: tool
name: curl
contributors:
    - ["Alyssa Evans", "https://github.com/alycda"]
filename: learncurl.sh
---

curl is a command-line tool for transferring data using various network protocols.
It supports HTTP, HTTPS, FTP, FTPS, SCP, SFTP, TFTP, DICT, TELNET, LDAP, and more.

curl is widely used for API testing, downloading files, and automating web interactions.
It's available on most Unix-like systems and Windows.

## Basic Usage

```bash
# Simple GET request - fetch a webpage
curl https://example.com

# Save output to a file with -o (lowercase o for custom name)
curl -o mypage.html https://example.com

# Save with remote filename using -O (uppercase O)
curl -O https://example.com/file.pdf

# Follow redirects with -L
curl -L https://example.com
# Many websites redirect HTTP to HTTPS, so -L is often needed

# Show response headers with -i (include)
curl -i https://example.com

# Show only response headers with -I (head request)
curl -I https://example.com

# Verbose output for debugging with -v
curl -v https://example.com
# Shows request headers, response headers, SSL handshake, etc.

# Silent mode - suppress progress bar with -s
curl -s https://example.com

# Show errors even in silent mode with -S
curl -sS https://api.example.com/data

# Download with progress bar
curl -# -O https://example.com/largefile.zip

# Modern readable syntax with double-dash (introduced in recent versions)
curl --head https://example.com        # Same as -I
curl --location https://example.com    # Same as -L
curl --output file.html https://example.com  # Same as -o
```

## Quick & Practical Examples

Before diving deeper, here are some fun and useful curl commands you can try right now:

```bash
# Get your public IP address
curl ifconfig.me
curl checkip.amazonaws.com
curl https://api.ipify.org

# Get weather forecast for your city
curl wttr.in/London
curl wttr.in/Tokyo
# Add ?format=3 for one-line output: curl wttr.in/London?format=3

# Get a random excuse
curl https://excuser-three.vercel.app/v1/excuse

# Unshorten/resolve a shortened URL (see where it redirects)
curl -sI https://bit.ly/short-url | grep -i location
# Or get the final URL after all redirects:
curl -Ls -o /dev/null -w %{url_effective} https://bit.ly/short-url

# Get word definition using dictionary protocol
# https://curl.se/mail/archive-2015-12/0011.html
# curl dict://dict.org/d:computer
# curl dict://dict.org/d:programming

# Check if a website is up (just get HTTP status code)
curl -s -o /dev/null -w "%{http_code}" https://example.com

# Currency conversion (if service is available)
curl "https://api.exchangerate-api.com/v4/latest/USD"

# Get your user agent string as seen by servers
curl https://httpbin.org/user-agent

# Pastebin-like sharing via curl
echo "Hello World" | curl -F 'file=@-' https://0x0.st
```

## Working with JSON responses (jq integration)

curl pairs perfectly with jq for parsing JSON responses:

```bash
# Install jq first: brew install jq (macOS) or apt-get install jq (Linux)

# Pretty print JSON response
curl -s https://api.github.com/users/octocat | jq '.'

# Extract specific fields
curl -s https://api.github.com/users/octocat | jq '.name'
curl -s https://api.github.com/users/octocat | jq '.name, .bio, .public_repos'

# Extract nested data
curl -s https://httpbin.org/headers | jq '.headers["User-Agent"]'

# Filter arrays
curl -s https://api.github.com/users/octocat/repos | jq '.[].name'

# Combine curl and jq for API automation
STARS=$(curl -s https://api.github.com/repos/curl/curl | jq '.stargazers_count')
echo "curl has $STARS stars on GitHub"
```

## HTTP Methods

```bash
# GET request (default)
curl https://api.example.com/users

# POST request with -X or --request
curl -X POST https://api.example.com/users

# PUT request
curl -X PUT https://api.example.com/users/123

# DELETE request
curl -X DELETE https://api.example.com/users/123

# PATCH request
curl -X PATCH https://api.example.com/users/123

# HEAD request (like -I but explicit)
curl -X HEAD https://api.example.com/status

# OPTIONS request (check allowed methods)
curl -X OPTIONS https://api.example.com/users
```

## Sending Data

```bash
# Send form data with -d or --data
curl -X POST -d "name=John&email=john@example.com" \
  https://api.example.com/users

# Send JSON data (specify Content-Type header)
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}' \
  https://api.example.com/users

# Read data from file with @
curl -X POST \
  -H "Content-Type: application/json" \
  -d @data.json \
  https://api.example.com/users

# URL encode data automatically with --data-urlencode
curl -X POST \
  --data-urlencode "message=Hello World! @2024" \
  https://api.example.com/messages

# Send form data as multipart/form-data with -F
curl -X POST \
  -F "name=John" \
  -F "email=john@example.com" \
  https://api.example.com/users

# Binary data with --data-binary
curl -X POST \
  --data-binary @image.png \
  https://api.example.com/upload
```

## Headers

```bash
# Add custom header with -H or --header
curl -H "User-Agent: MyApp/1.0" https://api.example.com

# Multiple headers
curl -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "X-API-Key: abc123" \
  https://api.example.com/data

# Remove a header with empty value
curl -H "User-Agent:" https://api.example.com
# This removes the default User-Agent header

# Common headers for APIs
curl -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Accept: application/json" \
  https://api.example.com/protected

# Set referrer
curl -H "Referer: https://example.com" \
  https://api.example.com/resource

# Custom Accept-Language
curl -H "Accept-Language: en-US,en;q=0.9" \
  https://api.example.com
```

## Authentication

```bash
# Basic authentication with -u or --user
curl -u username:password https://api.example.com

# Basic auth with prompt for password (more secure)
curl -u username https://api.example.com
# curl will prompt for password

# Bearer token authentication
curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  https://api.example.com/protected

# API key in header
curl -H "X-API-Key: YOUR_API_KEY" \
  https://api.example.com/data

# API key in query parameter
curl "https://api.example.com/data?api_key=YOUR_API_KEY"

# Digest authentication
curl --digest -u username:password \
  https://api.example.com

# NTLM authentication
curl --ntlm -u username:password \
  https://api.example.com

# AWS Signature Version 4 authentication (AWS SigV4)
# Authenticates with AWS services using IAM credentials
curl --aws-sigv4 "aws:amz:us-east-1:s3" \
  --user "$AWS_ACCESS_KEY_ID:$AWS_SECRET_ACCESS_KEY" \
  https://my-bucket.s3.us-east-1.amazonaws.com/file.txt

# AWS SigV4 example with EC2
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
curl --aws-sigv4 "aws:amz:us-east-1:ec2" \
  --user "$AWS_ACCESS_KEY_ID:$AWS_SECRET_ACCESS_KEY" \
  "https://ec2.us-east-1.amazonaws.com/?Action=DescribeInstances&Version=2016-11-15"

# Format: --aws-sigv4 "provider:service:region:service"
# Common providers: aws (Amazon), goog (Google), azure (Microsoft)
```

## Cookies

```bash
# Send cookies with -b or --cookie
curl -b "session=abc123" https://example.com

# Multiple cookies
curl -b "session=abc123; theme=dark" https://example.com

# Read cookies from file
curl -b cookies.txt https://example.com

# Save cookies to file with -c or --cookie-jar
curl -c cookies.txt https://example.com/login

# Use both: read and save cookies
curl -b cookies.txt -c cookies.txt https://example.com
# This maintains session across multiple requests

# Include session cookies in header manually
curl -H "Cookie: session=abc123; user_id=456" \
  https://example.com
```

## File Uploads

```bash
# Upload file with -F (multipart/form-data)
curl -X POST -F "file=@document.pdf" \
  https://api.example.com/upload

# Upload with custom filename
curl -X POST -F "file=@document.pdf;filename=newname.pdf" \
  https://api.example.com/upload

# Upload with MIME type specified
curl -X POST \
  -F "file=@photo.jpg;type=image/jpeg" \
  https://api.example.com/upload

# Multiple file upload
curl -X POST \
  -F "file1=@doc1.pdf" \
  -F "file2=@doc2.pdf" \
  https://api.example.com/upload

# Upload with additional form fields
curl -X POST \
  -F "file=@document.pdf" \
  -F "title=My Document" \
  -F "category=reports" \
  https://api.example.com/upload

# Upload raw file data
curl -X POST \
  --data-binary @file.zip \
  -H "Content-Type: application/zip" \
  https://api.example.com/upload

# Upload from stdin
echo "test data" | curl -X POST \
  --data-binary @- \
  https://api.example.com/upload
```

## Downloads

```bash
# Resume interrupted download with -C (continue-at) - ESSENTIAL for large files!
curl -C - -O https://example.com/largefile.zip
# -C - automatically determines where to resume
# If download stops at 65MB of 100MB, running again continues from 65MB

# Example: Resume a partially downloaded file
curl -C - -# -O https://releases.ubuntu.com/22.04/ubuntu-22.04-desktop-amd64.iso

# Download multiple files
curl -O https://example.com/file1.zip \
     -O https://example.com/file2.zip

# Parallel downloads with --parallel (curl 7.66.0+)
curl --parallel \
     -O https://example.com/file1.zip \
     -O https://example.com/file2.zip \
     -O https://example.com/file3.zip
# Downloads all files simultaneously instead of sequentially

# Parallel with maximum connections
curl --parallel --parallel-max 3 \
     -O https://example.com/file[1-10].zip
# Limits to 3 simultaneous downloads

# Download with URL globbing
curl -O https://example.com/file[1-10].jpg
# Downloads file1.jpg through file10.jpg

# Parallel globbing for faster batch downloads
curl --parallel -O https://example.com/image[1-20].jpg
# Downloads 20 images in parallel

# Download with custom output names using globbing
curl https://example.com/image[1-3].jpg -o "photo_#1.jpg"
# Saves as photo_1.jpg, photo_2.jpg, photo_3.jpg

# Limit download rate (bandwidth throttling)
curl --limit-rate 100K -O https://example.com/file.zip
# Limits to 100 KB/s (useful to avoid saturating connection)

# Range requests (partial content)
curl -r 0-999 https://example.com/file.bin
# Downloads first 1000 bytes

# Download with progress bar instead of meter
curl -# -O https://example.com/largefile.zip
# Shows: ###################################  45.2%

# Alternative: Use xargs for older curl versions without --parallel
cat urls.txt | xargs -P 4 -n 1 curl -O
# Downloads 4 files in parallel using xargs
```

## Timeouts and Retries

```bash
# Connection timeout with --connect-timeout
curl --connect-timeout 10 https://example.com
# Fails if connection takes more than 10 seconds

# Maximum time for entire operation with -m or --max-time
curl --max-time 60 https://example.com
# Fails if entire transfer takes more than 60 seconds

# Retry on failure with --retry
curl --retry 5 https://api.example.com
# Retries up to 5 times on transient errors

# Delay between retries
curl --retry 5 --retry-delay 3 \
  https://api.example.com
# Waits 3 seconds between retries

# Maximum retry time
curl --retry 5 --retry-max-time 60 \
  https://api.example.com
# Stops retrying after 60 seconds total
```

## SSL/TLS Options

```bash
# Ignore SSL certificate verification (insecure!)
curl -k https://self-signed.example.com
# or --insecure

# Specify CA certificate bundle
curl --cacert /path/to/ca-bundle.crt \
  https://example.com

# Use client certificate
curl --cert client.pem --key client-key.pem \
  https://api.example.com

# Show SSL certificate info
curl -v --insecure https://example.com 2>&1 | grep -A 10 "Server certificate"

# Specify TLS version
curl --tlsv1.2 https://example.com
curl --tlsv1.3 https://example.com

# Show supported TLS versions
curl --tls-max 1.2 https://www.howsmyssl.com/a/check
```

## Proxy Support

```bash
# Use HTTP proxy with -x or --proxy
curl -x http://proxy.example.com:8080 \
  https://api.example.com

# Proxy with authentication
curl -x http://user:pass@proxy.example.com:8080 \
  https://api.example.com

# Use SOCKS5 proxy
curl -x socks5://localhost:1080 \
  https://api.example.com

# Bypass proxy for specific hosts
curl -x http://proxy.example.com:8080 \
  --noproxy "localhost,127.0.0.1,.example.com" \
  https://api.example.com

# Use system proxy settings from environment
# curl automatically uses HTTP_PROXY, HTTPS_PROXY, and NO_PROXY
export HTTP_PROXY=http://proxy.example.com:8080
curl https://api.example.com
```

## Output Control

```bash
# Write output to file with -o
curl -o output.html https://example.com

# Append to file instead of overwrite
curl https://example.com >> output.html

# Save response headers to file
curl -D headers.txt https://example.com

# Save both headers and body
curl -i https://example.com -o response.txt

# Only show errors with -S in silent mode
curl -sS https://api.example.com

# Write to stdout even when saving
curl -o file.html https://example.com -w "\n"

# Format output with -w (write-out)
curl -w "HTTP: %{http_code}\nTime: %{time_total}s\n" \
  https://example.com -o /dev/null

# Common write-out variables:
# %{http_code}      - HTTP status code
# %{time_total}     - Total time in seconds
# %{time_connect}   - Time to establish connection
# %{size_download}  - Downloaded bytes
# %{speed_download} - Download speed
# %{url_effective}  - Final URL after redirects
```

## Advanced Features

```bash
# Follow redirects with maximum redirect count
curl -L --max-redirs 5 https://example.com

# Send referrer automatically on redirects
curl -L --referer ";auto" https://example.com

# Compressed responses (gzip, deflate)
curl --compressed https://example.com

# Keep-alive connections
curl --keepalive-time 60 https://example.com

# HTTP/2 support
curl --http2 https://example.com

# HTTP/3 support (if compiled with support)
curl --http3 https://example.com

# IPv4 only
curl -4 https://example.com

# IPv6 only
curl -6 https://example.com

# Resolve host to specific IP
curl --resolve example.com:443:93.184.216.34 \
  https://example.com

# Use specific interface/IP address
curl --interface eth0 https://example.com

# Custom DNS server
curl --dns-servers 8.8.8.8,8.8.4.4 \
  https://example.com

# HAProxy PROXY protocol header (for load balancers)
curl --haproxy-protocol https://example.com
# Sends PROXY protocol v1 header before HTTP request
# Used when connecting through HAProxy or similar load balancers
```

## Beyond HTTP: Other Protocols

curl supports 30+ protocols beyond HTTP. Here are some practical examples:

```bash
# Dictionary Protocol - Get word definitions
curl dict://dict.org/d:curl
curl dict://dict.org/d:HTTP
curl dict://dict.org/d:network

# Show available dictionaries
curl dict://dict.org/show:db

# MQTT - Publish/Subscribe messaging (curl 7.71.0+)
# Publish a message to MQTT broker
curl mqtt://test.mosquitto.org:1883/demo/topic \
  --data "Hello from curl"

# Subscribe to MQTT topic (requires keeping connection open)
curl mqtt://test.mosquitto.org:1883/demo/topic

# IMAP - Read emails
# List mailboxes
curl --user username:password \
  imaps://imap.gmail.com

# Check inbox
curl --user username:password \
  imaps://imap.gmail.com/INBOX

# Read specific email (message 1)
curl --user username:password \
  "imaps://imap.gmail.com/INBOX;UID=1"

# For Gmail, use app-specific password
curl --user "user@gmail.com:app-password" \
  "imaps://imap.gmail.com/INBOX?ALL"

# POP3 - Retrieve emails
curl --user username:password \
  pop3s://pop.gmail.com

# Get specific message
curl --user username:password \
  pop3s://pop.gmail.com/1

# SMTP - Send email
curl --mail-from sender@example.com \
  --mail-rcpt recipient@example.com \
  --upload-file email.txt \
  smtp://smtp.example.com:587 \
  --user username:password

# FTP - File transfer
# List directory
curl ftp://ftp.example.com/

# Download file
curl -O ftp://ftp.example.com/file.txt

# Upload file
curl -T localfile.txt ftp://ftp.example.com/ \
  --user username:password

# FTPS (FTP over SSL)
curl -T file.txt ftps://secure.ftp.com/ \
  --user username:password

# SFTP (SSH File Transfer Protocol)
curl -u username:password \
  sftp://example.com/path/to/file.txt

# SCP - Secure copy
curl -u username:password \
  scp://example.com/path/to/file.txt

# Telnet - Connect to telnet server
curl telnet://towel.blinkenlights.nl
# (Famous Star Wars ASCII animation)

# LDAP - Query directory services
curl "ldap://ldap.example.com/dc=example,dc=com"

# FILE - Read local files (yes, curl can read local files!)
curl file:///etc/hosts
curl file:///Users/username/document.txt

# TFTP - Trivial File Transfer Protocol
curl -T file.txt tftp://192.168.1.100/
```

## Performance Timing and Debugging

Get detailed performance metrics for every phase of a request:

```bash
# Quick timing - just total time
curl -w "Total time: %{time_total}s\n" -o /dev/null -s \
  https://example.com

# Detailed timing breakdown
curl -w "\n
DNS Lookup:        %{time_namelookup}s
TCP Connect:       %{time_connect}s
TLS Handshake:     %{time_appconnect}s
Pre-transfer:      %{time_pretransfer}s
Start Transfer:    %{time_starttransfer}s
Redirect:          %{time_redirect}s
                   ----------
Total:             %{time_total}s
" -o /dev/null -s https://example.com

# Create a reusable timing format file
cat > curl-timing.txt << 'EOF'
    time_namelookup:  %{time_namelookup}s\n
       time_connect:  %{time_connect}s\n
    time_appconnect:  %{time_appconnect}s\n
   time_pretransfer:  %{time_pretransfer}s\n
      time_redirect:  %{time_redirect}s\n
 time_starttransfer:  %{time_starttransfer}s\n
                    ----------\n
         time_total:  %{time_total}s\n
        size_download: %{size_download} bytes\n
       speed_download: %{speed_download} bytes/sec\n
          http_code:  %{http_code}\n
EOF

# Use the timing format file
curl -w "@curl-timing.txt" -o /dev/null -s https://api.github.com

# Measure API endpoint performance (10 requests average)
for i in {1..10}; do
  curl -w "%{time_total}\n" -o /dev/null -s https://api.example.com
done | awk '{sum+=$1; sumsq+=$1*$1} END {
  print "Average:", sum/NR, "s";
  print "Std Dev:", sqrt(sumsq/NR - (sum/NR)^2), "s"
}'

# All available write-out variables for debugging:
# %{content_type}       - Content-Type of response
# %{errormsg}           - Error message if failed
# %{exitcode}           - Numerical exit code
# %{filename_effective} - Final filename
# %{ftp_entry_path}     - Initial path for FTP
# %{http_code}          - HTTP status code
# %{http_connect}       - HTTP CONNECT response code
# %{http_version}       - HTTP version used
# %{local_ip}           - Local IP address
# %{local_port}         - Local port number
# %{method}             - HTTP method used
# %{num_connects}       - Number of connections made
# %{num_headers}        - Number of response headers
# %{num_redirects}      - Number of redirects followed
# %{proxy_ssl_verify_result} - Proxy SSL cert verification result
# %{redirect_url}       - URL of redirect
# %{referer}            - Referer header
# %{remote_ip}          - Remote IP address
# %{remote_port}        - Remote port number
# %{response_code}      - Response code
# %{scheme}             - URL scheme used
# %{size_download}      - Bytes downloaded
# %{size_header}        - Bytes of headers
# %{size_request}       - Bytes sent in request
# %{size_upload}        - Bytes uploaded
# %{speed_download}     - Download speed (bytes/sec)
# %{speed_upload}       - Upload speed (bytes/sec)
# %{ssl_verify_result}  - SSL cert verification result
# %{time_appconnect}    - Time for SSL/TLS handshake
# %{time_connect}       - Time to establish TCP
# %{time_namelookup}    - Time for DNS resolution
# %{time_pretransfer}   - Time before file transfer starts
# %{time_redirect}      - Time spent in redirects
# %{time_starttransfer} - Time to first byte
# %{time_total}         - Total operation time
# %{url}                - URL that was fetched
# %{url_effective}      - Final URL after redirects
# %{urlnum}             - URL index (for multiple URLs)
```

## Testing and Debugging

```bash
# Test API response time
curl -w "@curl-format.txt" -o /dev/null -s \
  https://api.example.com

# Where curl-format.txt contains:
#     time_namelookup:  %{time_namelookup}s\n
#        time_connect:  %{time_connect}s\n
#     time_appconnect:  %{time_appconnect}s\n
#    time_pretransfer:  %{time_pretransfer}s\n
#       time_redirect:  %{time_redirect}s\n
#  time_starttransfer:  %{time_starttransfer}s\n
#                     ----------\n
#          time_total:  %{time_total}s\n

# Trace all requests to file
curl --trace trace.txt https://api.example.com

# ASCII trace (more readable)
curl --trace-ascii trace.txt https://api.example.com

# Test HTTP status codes
curl -w "%{http_code}\n" -o /dev/null -s \
  https://example.com

# Test if URL is accessible (just status)
curl -I -s -o /dev/null -w "%{http_code}" \
  https://example.com

# HEAD request to check if resource exists
if curl -I -s -f -o /dev/null \
  "https://example.com/file.pdf"; then
  echo "File exists"
else
  echo "File not found"
fi

# Verbose SSL debugging
curl -v --insecure https://example.com 2>&1 | \
  grep -E "SSL|TLS|cipher"
```

## Configuration Files

```bash
# Use configuration file with -K or --config
curl -K config.txt

# Example config.txt contents:
# url = "https://api.example.com/users"
# header = "Authorization: Bearer TOKEN"
# header = "Content-Type: application/json"
# output = "users.json"

# Default config file location
# curl automatically reads from ~/.curlrc
# Example ~/.curlrc:
# silent
# show-error
# location
# compressed

# Ignore ~/.curlrc with -q
curl -q https://example.com
```

## Practical Examples

```bash
# API testing - POST JSON and check response
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}' \
  -w "\nHTTP: %{http_code}\n"

# Download file with progress and resume capability
curl -L -C - -# -o file.zip \
  https://example.com/downloads/file.zip

# Health check script
curl -sf https://api.example.com/health || \
  echo "Service is down!" | mail -s "Alert" admin@example.com

# Upload and get response
curl -X POST \
  -F "file=@document.pdf" \
  -F "metadata={\"title\":\"Report\",\"year\":2024};type=application/json" \
  https://api.example.com/documents

# Authenticated API call with error handling
response=$(curl -s -w "\n%{http_code}" \
  -H "Authorization: Bearer $TOKEN" \
  https://api.example.com/data)
http_code=$(echo "$response" | tail -n1)
body=$(echo "$response" | sed '$d')

if [ "$http_code" -eq 200 ]; then
  echo "Success: $body"
else
  echo "Error $http_code: $body"
fi

# GraphQL query
curl -X POST https://api.example.com/graphql \
  -H "Content-Type: application/json" \
  -d '{
    "query": "{ users { id name email } }"
  }'

# WebSocket upgrade request
curl -i -N \
  -H "Connection: Upgrade" \
  -H "Upgrade: websocket" \
  -H "Sec-WebSocket-Version: 13" \
  -H "Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==" \
  https://example.com/ws

# Test REST API CRUD operations
# Create
curl -X POST https://api.example.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","body":"Content"}'

# Read
curl https://api.example.com/posts/1

# Update
curl -X PUT https://api.example.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Updated","body":"New content"}'

# Delete
curl -X DELETE https://api.example.com/posts/1
```

## Common Use Cases

```bash
# Check if website is up
curl -Is https://example.com | head -n 1

# Get public IP address
curl https://ifconfig.me
curl https://api.ipify.org

# Get weather (wttr.in service)
curl https://wttr.in/London

# URL shortener
curl -s https://is.gd/create.php \
  -d "format=simple&url=https://very-long-url.example.com"

# Send Slack notification
curl -X POST https://hooks.slack.com/services/YOUR/WEBHOOK/URL \
  -H "Content-Type: application/json" \
  -d '{"text":"Deployment completed!"}'

# GitHub API - list repositories
curl -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/user/repos

# Download and pipe to another command
curl -s https://example.com/data.json | jq '.users[].name'

# Benchmark API endpoint (average of 10 requests)
for i in {1..10}; do
  curl -w "%{time_total}\n" -o /dev/null -s \
    https://api.example.com
done | awk '{sum+=$1} END {print "Average:",sum/NR,"s"}'
```

## Tips and Best Practices

```bash
# Always follow redirects for web scraping
curl -L https://example.com

# Use -f to fail silently on HTTP errors in scripts
curl -f https://api.example.com/data || echo "Request failed"

# Combine options for production use
curl -sSfL --retry 3 --max-time 30 \
  https://api.example.com

# Quote URLs with special characters
curl "https://example.com/search?q=hello world&lang=en"

# Use environment variables for sensitive data
curl -H "Authorization: Bearer $API_TOKEN" \
  https://api.example.com

# Save credentials in .netrc file for automatic auth
# ~/.netrc format:
# machine api.example.com
# login username
# password secret
curl -n https://api.example.com

# Use --fail-with-body to see error responses
curl --fail-with-body https://api.example.com/endpoint

# Test with different User-Agent strings
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" \
  https://example.com
```

## Further Reading

* [Official curl documentation](https://curl.se/docs/)
* [curl man page](https://curl.se/docs/manpage.html)
* [Everything curl book](https://everything.curl.dev/)
* [curl exercises](https://jvns.ca/blog/2019/08/27/curl-exercises/)
* [HTTP status codes reference](https://httpstatuses.com/)
