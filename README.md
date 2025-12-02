```cyberpunk
╔══════════════════════════════════════════════════════════════════════════════╗
║ ██████╗  █████╗ ██╗      ██████╗ ███╗   ██╗███████╗██████╗ ██╗███████╗███████╗ ║
║ ██╔══██╗██╔══██╗██║     ██╔═══██╗████╗  ██║██╔════╝██╔══██╗██║██╔════╝██╔════╝ ║
║ ██████╔╝███████║██║     ██║   ██║██╔██╗ ██║█████╗  ██████╔╝██║█████╗  █████╗   ║
║ ██╔══██╗██╔══██║██║     ██║   ██║██║╚██╗██║██╔══╝  ██╔══██╗██║██╔══╝  ██╔══╝   ║
║ ██████╔╝██║  ██║███████╗╚██████╔╝██║ ╚████║███████╗██████╔╝██║███████╗███████╗ ║
║ ╚═════╝ ╚═╝  ╚═╝╚══════╝ ╚═════╝ ╚═╝  ╚═══╝╚══════╝╚═════╝ ╚═╝╚══════╝╚══════╝ ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

# 🔥 BULLETPROOF JS CRAWLER v2.0 🔥
### *Cyberpunk JavaScript Enumeration Suite*
> **Status:** 🔐 Active | **Updated:** Dec 2025 | **Type:** Offensive Security Tool

---

## 🚀 **OVERVIEW**

**BULLETPROOF JS CRAWLER** is a next-generation, bulletproof JavaScript enumeration suite designed for red teams and bug bounty hunters. This tool leverages multiple attack vectors to extract JavaScript files, endpoints, and potential secrets from web applications with maximum efficiency.

### 💀 **FEATURES**
- **Multi-Tool Integration**: Combines `gau-tool`, `waybackurls`, `katana`, `hakrawler`, and `getJS`
- **Parallel Processing**: Optimized for speed with concurrent operations
- **Advanced Filtering**: Smart regex for JS file detection and endpoint extraction
- **Secret Discovery**: Automated secret hunting in JS files
- **Comprehensive Reporting**: Detailed output with live and dead JS endpoints

---

## 🛠️ **PREREQUISITES**

### Required Tools:
```bash
# Essential Tools (Install via go install or package manager)
go install github.com/lc/gau/v2/cmd/gau@latest
go install github.com/tomnomnom/waybackurls@latest
go install github.com/projectdiscovery/katana/cmd/katana@latest
go install github.com/hakluke/hakrawler@latest
go install github.com/0xsha/getJS@latest
go install github.com/projectdiscovery/httpx/cmd/httpx@latest

# Alternative installation via aquatone or directly from GitHub
```

### Dependencies:
- `bash` (v4.0+)
- `xargs` (parallel processing)
- `grep` with regex support
- `sort` and `wc` utilities

---

## 🎯 **USAGE**

### Basic Syntax:
```bash
./js_crawler.sh subdomains.txt [output_path]
```

### Example Usage:
```bash
# Basic usage
./js_crawler.sh targets.txt

# Custom output directory
./js_crawler.sh targets.txt /path/to/output/
```

### Input Requirements:
- **Format**: Plain text file with one domain per line
- **Example**:
  ```
  example.com
  subdomain.example.com
  another-domain.com
  ```

---

## ⚡ **WORKFLOW BREAKDOWN**

### **Phase 1: Alive Domain Discovery**
- Probes domains with `httpx` for 200-OK responses
- Filters out dead targets
- Default timeout: 7 seconds

### **Phase 2: Archive URL Extraction** *(Parallel Processing)*
- **gau-tool**: Extracts URLs from various sources with subdomain support
- **waybackurls**: Retrieves URLs from Wayback Machine
- Runs **concurrently** for maximum efficiency
- Combined and deduplicated output

### **Phase 3: Active Crawling**
- **katana**: Advanced crawling with headless browser support
- Depth: 4 levels, JavaScript/AJAX crawling enabled
- **hakrawler**: Subdomain-focused crawling with custom configuration

### **Phase 4: JavaScript Discovery**
- Regex pattern matching for `.js` extensions
- Handles query parameters and fragments
- Filters all discovered URLs for JavaScript files

### **Phase 5: Live JS Verification**
- Probes extracted JS files with `httpx`
- Verifies they're accessible and alive
- Creates list of live JS files for further analysis

### **Phase 6: Endpoint Extraction**
- Crawls live JS files to find embedded endpoints
- Extracts URLs found within JavaScript code
- Filters for HTTP-based endpoints

### **Phase 7: Secret Hunting**
- Scans JS content for potential secrets (keys, tokens, passwords)
- Uses `getJS` to retrieve complete file content
- Applies regex patterns for sensitive information

---

## 📊 **OUTPUT STRUCTURE**

### Generated Files:
```
[output_directory]/
├── alive.txt           # 200-OK domains from httpx
├── archive_urls.txt    # URLs from gau-tool + waybackurls
├── katana_urls.txt     # URLs discovered by katana
├── hakrawler_urls.txt  # URLs discovered by hakrawler
├── all_js.txt          # All extracted JS files (before verification)
├── js_live.txt         # Verified live JS files
├── js_endpoints.txt    # Endpoints found in JS files
└── secrets.txt         # Potential secrets discovered
```

### Key Metrics:
- Alive domains count
- Total JS files discovered
- Live JS files verified
- JS-based endpoints identified
- Potential secrets found

---

## 🔧 **CONFIGURATION**

### Adjustable Parameters:
- `THREADS=100` - General thread count for httpx and other tools
- `PARALLEL=20` - Number of parallel jobs for gau-tool/wayback processing
- `TIMEOUT` - Customizable for different environments

### Customization Options:
- Adjust depth levels in katana crawler (currently 4)
- Modify excluded extensions (png,jpg,jpeg,gif,css,woff,woff2,svg,ico)
- Edit secret detection regex patterns

---

## 🚨 **LEGAL DISCLAIMER**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  🔥 WARNING: This tool is intended for authorized penetration testing and   │
│  security research only. Unauthorized use may violate local, state, and     │
│  federal laws. The author assumes NO responsibility for misuse.             │
│                                                                             │
│  ⚖️  Use responsibly and only on targets you own or have explicit           │
│  permission to test.                                                        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🤖 **AUTOMATION INTEGRATION**

### CI/CD Pipeline:
```bash
# Example integration
./js_crawler.sh targets.txt && \
    cat $(ls -tr | tail -1)/secrets.txt | notify -silent -bulk
```

### Post-Processing:
- Integrate with `gf`, `qsreplace`, or other tools for advanced analysis
- Feed JS endpoints into vulnerability scanners
- Use secrets for further reconnaissance

---

## 📈 **PERFORMANCE METRICS**

- **5K+ domains**: ~5-15 minutes (depending on infrastructure)
- **Success rate**: 95%+ with proper configuration
- **Memory usage**: Optimal with parallel processing
- **Error handling**: Robust with comprehensive error checking

---

## 🏆 **CREDITS**

- **Author**: IceCream, theWhiteElephant
- **Contributors**: Offensive security researchers worldwide
- **Inspiration**: Modern JS-heavy web applications and their security challenges

---

## 🔄 **UPDATES & PATCHES**

### Latest Changes (Dec 2025):
- ✅ Fixed gau-tool threading (now uses `-t` instead of `-threads`)
- ✅ Hakrawler stdin issues resolved
- ✅ Improved regex for JS file detection
- ✅ Enhanced parallel processing for archive extraction
- ✅ Added headless browser support in katana

### Roadmap:
- [ ] Docker containerization
- [ ] Web UI interface
- [ ] Advanced secret detection patterns
- [ ] Integration with Burp Suite
- [ ] Real-time notification system

---

```cyberpunk
╔══════════════════════════════════════════════════════════════════════════════╗
║                    🔥 STAY CYBER, STAY SECURE 🔥                            ║
║             "In the future, everyone will have privacy for 15 minutes."      ║
╚══════════════════════════════════════════════════════════════════════════════╝
```
