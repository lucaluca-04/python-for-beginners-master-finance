# SSH Push Command

## Description
Push local files or code to a remote server via SSH. This command handles secure file transfer and code deployment to remote servers.

## Usage
This command will help you:
- Connect to remote servers via SSH
- Push local files/directories to remote server
- Deploy code to production/staging environments
- Sync project files to remote locations

## Default Destination
**GitHub Repository**: `https://github.com/lucaluca-04/python-for-beginners-master-finance`

## Parameters
When executing this command, you may be asked for:
- **Remote server address**: e.g., `example.com`, `192.168.1.100`, or `user@server.com`
- **GitHub repository**: `https://github.com/lucaluca-04/python-for-beginners-master-finance` (default)
- **Username**: SSH username (if not included in server address)
- **Remote path**: Destination path on remote server (e.g., `/var/www/app/`, `~/projects/`)
- **Local path**: Source path on local machine (default: current directory)
- **SSH key path**: Path to SSH private key (optional, uses default `~/.ssh/id_rsa` if not provided)
- **Port**: SSH port (default: 22)

## Common Use Cases

### 0. Push to GitHub Repository (Recommended)
```bash
# Push to GitHub using Git (most common method)
git remote add origin https://github.com/lucaluca-04/python-for-beginners-master-finance.git
git branch -M main
git push -u origin main

# Or if remote already exists, just push
git push origin main

# Using SSH URL (if SSH key is configured)
git remote set-url origin git@github.com:lucaluca-04/python-for-beginners-master-finance.git
git push origin main
```

### 1. Push entire project to remote server
```bash
# Push current directory to remote server
rsync -avz --exclude '.git' --exclude '__pycache__' --exclude '*.pyc' \
  -e "ssh -p 22" \
  ./ user@server.com:/path/to/destination/
```

### 2. Push specific files/directories
```bash
# Push specific files
scp -r /local/path/file.py user@server.com:/remote/path/

# Push directory
scp -r /local/directory/ user@server.com:/remote/path/
```

### 3. Push with SSH key
```bash
# Using specific SSH key
rsync -avz -e "ssh -i ~/.ssh/my_key -p 22" \
  ./ user@server.com:/path/to/destination/
```

### 4. Push and execute remote commands
```bash
# Push files and run deployment script
rsync -avz ./ user@server.com:/path/to/destination/ && \
ssh user@server.com "cd /path/to/destination && ./deploy.sh"
```

## Security Best Practices
- Always use SSH keys instead of passwords when possible
- Use `-p` flag to specify non-standard SSH ports
- Exclude sensitive files (`.env`, `.git`, etc.) from transfers
- Verify remote server identity before first connection
- Use `rsync` with `--dry-run` flag to preview changes before actual transfer

## Example Workflow

### For GitHub Push:
1. Check git status: `git status`
2. Add changes: `git add .`
3. Commit changes: `git commit -m "Update files"`
4. Push to GitHub: `git push origin main`
5. Verify on GitHub: Visit `https://github.com/lucaluca-04/python-for-beginners-master-finance`

### For SSH Server Push:
1. Check what will be transferred: `rsync -avz --dry-run ./ user@server.com:/path/`
2. Review the output
3. Execute actual transfer: `rsync -avz ./ user@server.com:/path/`
4. Verify files on remote server: `ssh user@server.com "ls -la /path/"`

## Quick Reference for GitHub Push

```bash
# Initialize git (if not already done)
git init

# Add remote repository
git remote add origin https://github.com/lucaluca-04/python-for-beginners-master-finance.git

# Or use SSH URL
git remote add origin git@github.com:lucaluca-04/python-for-beginners-master-finance.git

# Add all files
git add .

# Commit changes
git commit -m "Your commit message"

# Push to GitHub
git push -u origin main
```

## Notes
- **For GitHub**: Use `git push` instead of SSH file transfer (recommended)
- Use `rsync` for efficient incremental updates to servers (only transfers changed files)
- Use `scp` for simple one-time file transfers to servers
- Always backup remote files before overwriting
- Consider using deployment tools (Ansible, Fabric) for complex deployments
- GitHub repository: `https://github.com/lucaluca-04/python-for-beginners-master-finance`