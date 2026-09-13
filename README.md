# Cybersecurity Command Reference

A practical, free command reference for cybersecurity engineers, SOC analysts, IT administrators, and home-lab learners.

Use it as a quick reminder—not as a substitute for authorization. Run security commands only on systems you own or are explicitly allowed to test.

## Contents

- [Linux triage](#linux-triage)
- [Networking](#networking)
- [DNS and web](#dns-and-web)
- [Logs and processes](#logs-and-processes)
- [File and secret checks](#file-and-secret-checks)
- [Safe defensive scanning](#safe-defensive-scanning)
- [Free learning references](#free-learning-references)

## Linux triage

```bash
id
uname -a
ss -tulpn
systemctl --type=service --state=running
sudo journalctl -u ssh --since "24 hours ago"
sudo last -a | head -20
df -h
free -h
ps aux --sort=-%cpu | head -15
```

## Networking

```bash
ip addr
ip route
ip neigh
ping -c 4 example.com
curl -I https://example.com
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -dates -issuer -subject
```

## DNS and web

```bash
dig A example.com
dig MX example.com
dig TXT example.com
curl -sS -D - -o /dev/null https://example.com
curl -sS -L -o /dev/null -w '%{url_effective}\n' https://example.com
```

> Replace `example.com` with a domain you are authorized to inspect.

## Logs and processes

```bash
sudo journalctl --since "1 hour ago" | grep -Ei 'failed|invalid|sudo|denied' | tail -50
find /var/www -type f -mmin -120 -print 2>/dev/null
pstree -ap
sudo lsof -p <PID>
```

## File and secret checks

```bash
stat important-file
sha256sum important-file
find ./test-data -type f -perm -0002 -print
rg -n -i --glob '!node_modules' --glob '!\.git' '(api[_-]?key|secret|password|token)' .
```

Do not paste real secrets into tickets, chat, or public repositories. Rotate a credential if it was exposed.

## Safe defensive scanning

```bash
python -m compileall .
pip-audit
trivy image my-image:latest
```

For network scanners, document approved scope and rate limits first. Use commands only on systems you own or are explicitly allowed to test.

## Incident response mini-checklist

1. Preserve evidence and note the time zone.
2. Confirm scope: host, account, application, and suspected time window.
3. Contain carefully; do not destroy logs or reboot without a reason.
4. Rotate exposed credentials and revoke active sessions.
5. Record commands, outputs, decisions, and approvals.
6. Rebuild from a known-good source when integrity is uncertain.
7. Write a short lessons-learned note and add a preventive control.

## Free learning references

- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [MITRE D3FEND](https://d3fend.mitre.org/)
- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
- [CIS Controls](https://www.cisecurity.org/controls)
- [SANS Reading Room](https://www.sans.org/white-papers/)
- [Linux man-pages](https://man7.org/linux/man-pages/)
- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [Google Gruyere safe web security lab](https://google-gruyere.appspot.com/)

## Contributing

Add a short, safe, verifiable command. Explain it, preserve authorization warnings, and test Markdown before submitting.

## Disclaimer

Commands can be disruptive or expose sensitive data. Validate them in a lab, use least privilege, and obtain written authorization before testing systems you do not own.
