# BugBountyTips

<details open>
<summary><b>unauthenticated-jenkins-rce.yaml</b></summary>
  
```
curl -s "https://raw.githubusercontent.com/rix4uni/scope/refs/heads/main/data/Wildcards/inscope_wildcards.txt" | sed 's/^\*\.//' | sed 's/^\*//' | sed 's/^\.//' | grep -v "*" | tldinfo --silent --extract domain,suffix | unew -q wildcards.txt
```

```
[{
  "requirements": [
    { "bin": "httpx", "install": "gocl -i github.com/projectdiscovery/httpx" },
    { "bin": "nuclei", "install": "go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest" },
    { "bin": "notify", "install": "go install -v github.com/projectdiscovery/notify/cmd/notify@latest" },
    { "bin": "unew", "install": "gocl -i github.com/rix4uni/unew" }
  ],
  "command": [
    "for TARGET in $(cat ${INPUT});do curl -s \"https://raw.githubusercontent.com/rix4uni/BugBountyData/refs/heads/main/data/${TARGET}.txt\" | unew -q $TARGET.subs; cat $TARGET.subs | httpx -duc -silent -nc | unew -q $TARGET.httpx; wget -q https://raw.githubusercontent.com/rix4uni/custom-nuclei-templates/refs/heads/main/unauthenticated-jenkins-rce.yaml; cat $TARGET.httpx | nuclei -duc -silent -nc -t unauthenticated-jenkins-rce.yaml | unew $TARGET.${OUTPUT} | notify -duc -silent -id allvuln &>/dev/null; rm -rf $TARGET.subs $TARGET.httpx;done"
  ],
  "example": "garudrecon workflow kpt-jenkins -i wildcards.txt -o jenkins",
  "description": "Cheking Jenkins RCE using nuclei template"
}]
```
</details>
