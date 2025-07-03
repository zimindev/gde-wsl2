## 🐧 **WSL2 (Windows Subsystem for Linux) Setup Guide**  

### ✅ **What is WSL2?**  
**WSL2** lets you run a **full Linux environment** (Ubuntu, Debian, etc.) directly on Windows 10/11 **without dual-booting or VMs**. Key features:  
✔ **Faster than WSL1** (real Linux kernel)  
✔ **Full system call compatibility** (Docker, Kubernetes, ZFS work)  
✔ **GPU acceleration** (CUDA, ML workloads)  
✔ **Seamless Windows/Linux file access**  

---

## 🛠️ **Step 1: Install WSL2**  

### **Prerequisites**  
- Windows 10 (2004+) or Windows 11  
- Virtualization enabled in BIOS  

### **1. Enable WSL & Virtualization**  
Run in **PowerShell (Admin)**:  
```powershell
wsl --install
```
This automatically:  
- Enables WSL/VM platform features  
- Installs Ubuntu by default  

**Manual install (if needed):**  
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
**Restart your PC.**  

### **2. Set WSL2 as Default**  
```powershell
wsl --set-default-version 2
```

### **3. Install a Linux Distro**  
From **Microsoft Store**:  
- [Ubuntu](https://apps.microsoft.com/store/detail/ubuntu/9PDXGNCFSCZV)  
- [Debian](https://apps.microsoft.com/store/detail/debian/9MSVKQC78PK6)  
- [Kali Linux](https://apps.microsoft.com/store/detail/kali-linux/9PKR34TNCV07)  

Or via CLI:  
```powershell
wsl --install -d Ubuntu-22.04
```

---

## ⚙️ **Step 2: Basic Configuration**  

### **Launch Your Distro**  
- Open **Start Menu** → Search "Ubuntu"  
- Set up username/password  

### **Update Packages**  
```bash
sudo apt update && sudo apt upgrade -y
```

### **Access Windows Files**  
Windows drives are mounted at:  
```bash
/mnt/c/Users/YourName  # C:\Users\YourName
/mnt/d/                # D:\
```

### **Run GUI Apps (Optional)**  
Install a **X11 server** like [VcXsrv](https://sourceforge.net/projects/vcxsrv/) or use **WSLg** (built-in in Win11):  
```bash
sudo apt install gedit -y
gedit
```

---

## 🐋 **Step 3: Docker & Dev Tools**  

### **Install Docker in WSL2**  
```bash
sudo apt install docker.io
sudo service docker start
```

### **Or Use Docker Desktop (Recommended)**  
1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)  
2. Enable **WSL2 Integration**:  
   - Settings → Resources → WSL Integration → Enable for your distro  

### **Common Dev Tools**  
```bash
# Zsh + Oh My Zsh
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Python/Node
sudo apt install python3 pip nodejs npm
```

---

## 🔧 **Step 4: Advanced Setup**  

### **Change WSL Version**  
```powershell
wsl --set-version Ubuntu-22.04 2  # Convert to WSL2
wsl --set-version Ubuntu-22.04 1  # Downgrade to WSL1
```

### **Move WSL to Another Drive**  
1. Export:  
   ```powershell
   wsl --export Ubuntu-22.04 D:\wsl-ubuntu.tar
   ```
2. Import:  
   ```powershell
   wsl --import Ubuntu-22.04 D:\wsl\ D:\wsl-ubuntu.tar --version 2
   ```

### **GPU Acceleration (CUDA/PyTorch)**  
1. Install [NVIDIA drivers](https://developer.nvidia.com/cuda/wsl)  
2. In WSL:  
   ```bash
   sudo apt install nvidia-cuda-toolkit
   nvidia-smi  # Verify
   ```

---

## 📂 **File System Tips**  

| Task                      | Command                          |  
|--------------------------|----------------------------------|  
| Open WSL in Explorer      | `explorer.exe .`                 |  
| Open Windows in WSL       | `cd /mnt/c/Users/`               |  
| Edit Windows files safely | Use VS Code (`code .`)           |  

⚠ **Don’t modify Linux files from Windows** (corruption risk). Use `/mnt/` for cross-OS work.  

---

## 🚀 **Step 5: VS Code Integration**  

1. Install [WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)  
2. Open folder in WSL:  
   ```bash
   code .
   ```
3. **Full IDE features** (debugging, terminals, extensions) work in Linux!  

---

## 📚 **Troubleshooting**  

| Issue                     | Fix                              |  
|--------------------------|----------------------------------|  
| `wsl --install` fails     | Enable Windows Update + BIOS virtualization |  
| Slow file access          | Store projects in WSL (`~/`)     |  
| DNS not working           | Edit `/etc/wsl.conf`:            |  
|                          | ```ini                           |  
|                          | [network]                        |  
|                          | generateResolvConf = false       |  
|                          | ```                              |  

---

## 🔥 **Pro Tips**  
- **Systemd support (WSL2):** Edit `/etc/wsl.conf`:  
  ```ini
  [boot]
  systemd=true
  ```
- **Run Windows apps from WSL:**  
  ```bash
  notepad.exe
  ```
- **Clipboard integration:**  
  ```bash
  cat file.txt | clip.exe
  ```
