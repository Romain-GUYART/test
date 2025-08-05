#!/bin/bash

SERVER="http://127.0.0.1"

echo "🔎 Test 1 : Injection XSS"
curl -s -o /dev/null -w "%{http_code}\n" "$SERVER/?test=<script>alert(1)</script>"

echo "🔎 Test 2 : Injection SQL"
curl -s -o /dev/null -w "%{http_code}\n" "$SERVER/?id=1' OR '1'='1"

echo "🔎 Test 3 : Remote Command Execution"
curl -s -o /dev/null -w "%{http_code}\n" "$SERVER/?cmd=cat /etc/passwd"

echo "🔎 Test 4 : Path Traversal"
curl -s -o /dev/null -w "%{http_code}\n" "$SERVER/?file=../../../../etc/shadow"

echo "🔎 Test 5 : User-Agent malveillant"
curl -s -A "sqlmap" -o /dev/null -w "%{http_code}\n" "$SERVER/"

echo "✅ Tests terminés. Vérifie les codes HTTP ou consulte les logs (/var/log/modsec_audit.log)"
