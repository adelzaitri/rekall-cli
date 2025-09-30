# Rekall-CLI

**Pentest Command Recall Tool**

A fast CLI utility to recall, search, and organize penetration-testing commands with JSON-based entries and fuzzy search.  
Useful for OSCP students, red teamers, and anyone who wants a portable cheat-sheet of tools & commands.

---



---

## Usage

```bash
rekall list               # list available tools
rekall search <query>     # tool search (deterministic substring)
rekall search -f <query>  # fuzzy tool search (uses rapidfuzz)
rekall search <port>      # search by port number (e.g., 22, 445, 3389)
rekall show <id|prefix|tool>  # show full entry
```
## Examples
```
# list everything
rekall list

# exact/deterministic search
rekall search nmap

# fuzzy search
rekall search -f rustscan

# search by port number (e.g., 22 → SSH tools)
rekall search 22

# show full entry
rekall show burpsuite
rekall show 11f7f531-a67
```

## Example output:
```
11f7f531-a67  100%  nmap/recon  - Nmap - fast & service scans (2 cmds)
    Flexible network scanner for port discovery, service/version detection and scripting.
    [1] Fast SYN all ports
        command: nmap -sS -T4 -p- --min-rate 1000 --open -oN nmap_all_{TARGET}.txt {TARGET}
        desc:    Fast SYN scan across all ports and save output.
    [2] Service/version + default scripts
        command: nmap -sV -sC -p 22,80,443 -oN nmap_services_{TARGET}.txt {TARGET}
        desc:    Service detection on common ports and run default NSE scripts.

```

## Quick Install (Linux)
```
# 1) Ensure python and venv tools are available (Debian/Ubuntu example)
sudo apt update
sudo apt install -y python3 python3-venv python3-pip

# 2) Create and activate a virtual environment
python3 -m venv ~/.venvs/rekall
source ~/.venvs/rekall/bin/activate

# 3) Upgrade pip and install dependencies
pip install --upgrade pip
pip install rapidfuzz

```

## Verify installation:
```
python -c "import rapidfuzz; print('rapidfuzz', rapidfuzz.__version__)"

```

## Example JSON Entry
Each tool lives as its own JSON file under commands/<category>/.
Here’s an example entry for Nmap:

```
{
  "id": "11f7f531-a67",
  "name": "Nmap - fast & service scans",
  "tool": "nmap",
  "category": "recon",
  "platform": "any",
  "tags": ["nmap", "recon", "scan"],
  "description": "Flexible network scanner for port discovery, service/version detection and scripting.",
  "ports": ["22", "80", "443", "3389", "445"],
  "commands": [
    {
      "name": "Fast SYN all ports",
      "command": "nmap -sS -T4 -p- --min-rate 1000 --open -oN nmap_all_{TARGET}.txt {TARGET}",
      "description": "Fast SYN scan across all ports and save output.",
      "ports": []
    },
    {
      "name": "Service/version + default scripts",
      "command": "nmap -sV -sC -p 22,80,443 -oN nmap_services_{TARGET}.txt {TARGET}",
      "description": "Service detection on common ports and run default NSE scripts.",
      "ports": ["22", "80", "443"]
    }
  ],
  "created": "2025-09-23T00:00:00Z",
  "modified": "2025-09-23T00:00:00Z"
}
```

