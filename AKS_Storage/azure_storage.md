<!-- What is Azure Files? -->
# What is Azure Files?
- Azure Files is a cloud-based file sharing service provided by Microsoft Azure.
- It lets you create a shared folder in the cloud, which can be accessed just like a regular drive or folder on your computer.

<!-- Think of it like this: -->
## Think of it like this:
Imagine a shared network folder in an office, where multiple people can upload, read, or edit files. Now imagine that folder is in the cloud—always available and accessible from anywhere. That’s Azure Files.

<!-- Key Features: -->
## Key Features:
- File Sharing – Share files between machines or apps using standard file protocols (like SMB or NFS).
- Mount Like a Drive – You can mount it as a drive (like "Z:") on Windows, Linux, or macOS.
- Cloud Native – It’s managed by Azure, no hardware or server needed.
- Secure – Supports encryption, authentication, and access control.

<!-- What is it used for? -->
## What is it used for?
- Hosting shared files (documents, media, config files)
- Lifting and shifting legacy apps that expect file storage
- Application logs or backups
- Container volumes (Kubernetes can mount Azure Files)

<!-- Types of Azure File Storage: -->
## Types of Azure File Storage:
- Standard File Shares – Backed by hard disks (good for general use).
- Premium File Shares – Backed by SSDs (good for high performance needs).

<!-- How to Use It: -->
## How to Use It:
- Create a Storage Account in Azure.
- Inside it, create a File Share.
- Get the connection string or URL.
- Mount it to your machine or app.