docker
- Do not execute as root. Set user and copy with chown
- Add sha hashes for base images
- Use version and sha for both system and app dependencies
- Copy dependencies, install, then source code
- Add --platform to validate build step
- Scan image for vulnerabilities
- Add data.csv at run time
- Use debian for reproducible images first, then change to

enclaves 
- Add verification of IAM instance role using PCR3 and instance ID PCR4
