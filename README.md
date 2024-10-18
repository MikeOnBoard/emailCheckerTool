# **Email Checker Tool GO**
## **Project Structure**
```plaintext
emailCheckerTool/
│
├── main.go            # Main application that checks domain email settings (MX, SPF, DMARC)
├── Dockerfile         # Docker setup for containerizing the tool
├── .env               # Environment variables (not pushed to the repo)
└── README.md          # Documentation
```
## **Documentation**
### **Project Overview**
**emailCheckerTool** is a simple command-line tool written in Go. It allows users to check the DNS records of a domain to verify if it has Mail Exchange (MX), Sender Policy Framework (SPF), and Domain-based Message Authentication, Reporting & Conformance (DMARC) records.

These records are essential for email validation, preventing spoofing, and ensuring that a domain’s email infrastructure is properly configured.

### **Prerequisites**
- Go installed on your machine
- Basic understanding of DNS and email protocols
- Docker (optional, if you want to run in a containerized environment)
### **Installation**
To install and run the project locally, clone the repository and build it:

```bash
git clone https://github.com/MikeOnBoard/emailCheckerTool.git
cd emailCheckerTool
go build -o emailCheckerTool main.go
```
You can now run the tool:

```bash
echo "example.com" | ./emailCheckerTool
```
This will check the DNS settings of ``example.com``.

### **Docker**
If you prefer to use Docker, you can build the tool into a container:

```bash
docker build -t emailchecker .
```
Run the Docker container, providing the domain list:

```bash
docker run -i emailchecker < domains.txt
```
### Environment Variables
Though the ``.env`` file is not pushed to the repository, it can be used to store any future environment-specific variables (none are required in the current version).

### Function Overview
- ``main()``: This is the entry point of the program. It reads domain names from stdin, one per line, and calls the ``checkDomain()`` function to inspect the DNS records for each domain.

- ``checkDomain(domain string)``: This function performs the DNS lookup for MX, SPF, and DMARC records:

  - **MX Records**: It checks if Mail Exchange records exist for the domain, indicating valid email routing.
  - **SPF Records**: It looks for SPF records that indicate which IP addresses are authorized to send emails on behalf of the domain.
  - **DMARC Records**: It checks for DMARC records to see if the domain has email protection against spoofing.
The function outputs the result in CSV format with columns for domain, MX status, SPF status, SPF record, DMARC status, and DMARC record.

### **Example Output**
When you run the program, it generates an output similar to:

```plaintext
domain, hasMX, hasSPF, spfRecord, hasDMARC, dmarcRecord
example.com, true, true, "v=spf1 include:_spf.example.com ~all", true, "v=DMARC1; p=none; rua=mailto:dmarc@example.com"
```
This gives a clear overview of the email configuration status for the given domain.

### Error Handling
- If there is an issue with the DNS lookup (e.g., the domain doesn’t exist), an error message will be logged, and the program will continue checking other domains.

## Conclusion
emailCheckerTool is a straightforward and efficient utility for validating a domain's email infrastructure by checking MX, SPF, and DMARC DNS records. This tool helps users ensure that domains are properly configured to handle email traffic and prevent spoofing, supporting stronger email security practices. The command-line interface provides quick insights into email settings, making it useful for system administrators, security professionals, and anyone managing domain email configurations.

The project is easy to deploy locally or via Docker, making it adaptable to various environments.