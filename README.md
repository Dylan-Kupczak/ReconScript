# ReconScript

## Overview

This is a ZSH script designed for initial reconnaissance in the context machine resolution. It automates detailed Nmap scanning, generates an HTML report, detects web services, and performs subdomain (virtual host) and directory fuzzing using FFUF when web servers are identified. The script is intended for ethical hacking and penetration testing purposes only, in controlled environments like HTB.

The script assumes the target is a domain or hostname (e.g., `machine.htb`) that has been added to `/etc/hosts` for resolution.

## Features

- **Detailed Nmap Scan**: Runs an aggressive Nmap scan with script scanning (`-sC`), version detection (`-sV`), OS detection (`-O`), full port range (`-p-`), and timing template 4 (`-T4`). Outputs to XML, normal, and grepable formats, with XML converted to HTML for easy viewing.
- **Web Service Detection**: Parses Nmap results to identify open HTTP/HTTPS ports.
- **Subdomain Fuzzing**: Uses FFUF for virtual host discovery with a top 1 million subdomains wordlist. Filters out common false positives (e.g., 404/403 responses).
- **Directory Fuzzing**: Uses FFUF to brute-force hidden directories and files with extensions `.js`, `.html`, `.php` on each detected web port. Supports HTTP and HTTPS based on port.
- **Output Organization**: Creates a dedicated directory for the target, storing all scan results, reports, and fuzzing outputs (text and JSON formats).

## Requirements

- **Shell**: ZSH (Z Shell)
- **Tools**:
  - `nmap`: For scanning.
  - `xsltproc`: For converting Nmap XML to HTML.
  - `ffuf`: For fuzzing (install via `go install github.com/ffuf/ffuf/v2@latest` or package manager).
  - `seclists`: Wordlists must be installed at `/usr/share/seclists/` (install via `apt install seclists` on Debian-based systems or equivalent).
- **Permissions**: Run as a user with access to the tools and wordlist paths. No root required unless scanning privileged ports.
- **Environment**: Tested on Kali Linux or similar pentesting distributions. Ensure the target is resolvable (e.g., via `/etc/hosts`).

## Installation

1. Clone the repository:
2. Make the script executable: chmox +x ReconScript.sh

## Usage

Run the script with a single argument: the target domain or hostname.
./ReconScript.sh <target></target>

Example:
./ReconScript.sh machine

### Output

- A directory named `<target>-recon/` will be created containing:
  - `nmap.html`: Interactive HTML Nmap report.
  - `nmap.xml`, `nmap.nmap`, `nmap.gnmap`: Raw Nmap outputs.
  - `subdomains.txt` and `subdomains.json`: Subdomain fuzzing results.
  - `directories_port_<port>.txt` and `directories_port_<port>.json`: Directory fuzzing results per port.
- Console output provides progress and summaries.

To view the Nmap HTML report:
open nmap.html  # Or use your preferred browser


## Customization

- **Wordlists**: Paths are hardcoded to SecLists defaults. Edit the script to change them.
- **FFUF Options**: Adjust filters (`-fc`), extensions (`-e`), or other flags as needed for specific targets.
- **Nmap Flags**: Modify the Nmap command for less/more aggression (e.g., add `--min-rate=1000` for faster scans).


## Disclaimer

This tool is for educational and ethical purposes only. Use responsibly and legally. The author is not responsible for misuse.
