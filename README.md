### Backend API Deployment Using GitHub Actions

---

## **Project Overview**

This project sets up and deploys a Flask-based backend API to a virtual machine (VM) using GitHub Actions. The API listens on **port 80** and exposes a single endpoint `/sayHello` that returns the JSON response:

```json
{"message": "Hello User"}
```

The process is automated, leveraging **GitHub Actions workflows** to deploy the API whenever code changes are pushed to the repository.

---

## **Workflow Explanation**

### **1. API Design**
- The backend API is built using Flask for its simplicity and lightweight design.
- The `/sayHello` endpoint responds to HTTP GET requests with a JSON message.
- The app is configured to run on **port 80**, enabling easy accessibility via the VM’s public IP address.

---

### **2. Deployment Process**
The deployment is managed by **GitHub Actions**, a CI/CD automation tool. The workflow ensures:
- The API code is pushed to the repository.
- The virtual machine (VM) is configured and the application is deployed and started.

Steps in the workflow:
1. **Trigger Event**: 
   - The workflow is triggered on pushes to the repository's default branch.
2. **Environment Setup**:
   - Python and Flask are installed on the VM, ensuring necessary dependencies are present.
3. **App Deployment**:
   - The application is deployed to the VM, configured to run in the background on port 80.

---

### **3. GitHub Secrets**
Sensitive credentials are securely managed through **GitHub Secrets**:
- **SSH_PRIVATE_KEY**: The private SSH key used to access the VM securely.
- These secrets are stored under **Settings > Secrets and Variables > Actions** in your repository.

---

### **4. Virtual Machine Details**
- **Host**: `20.127.206.174`
- **Username**: `azureuser`
- The VM is set up with SSH access and configured to allow deployment through the GitHub Actions workflow.

On the VM:
- Dependencies (e.g., Python, pip, Flask) are installed if missing.
- A virtual environment isolates the app's dependencies.
- The app is started and runs on port 80.

---

### **5. CI/CD Workflow**
The `.github/workflows/deploy.yml` file defines the deployment pipeline:
- **SSH to VM**: Uses the SSH private key from GitHub Secrets to log in.
- **Setup Environment**: Installs Flask and other required dependencies inside a Python virtual environment.
- **Deploy App**: Starts the Flask app using the `nohup` command to ensure it runs in the background.

---

### **6. Testing**
To verify the deployment:
1. Use `curl` or a browser to test the `/sayHello` endpoint:
   ```bash
   curl http://20.127.206.174/sayHello
   ```
2. Confirm the response:
   ```json
   {"message": "Hello User"}
   ```

---

## **Troubleshooting**

### **Common Issues**
1. **SSH Access Denied**:
   - Ensure the correct SSH key is configured in both the VM and GitHub Secrets.
   - Confirm the key permissions are set to `600` on the VM.

2. **App Not Running**:
   - Check if Flask is installed correctly by activating the virtual environment:
     ```bash
     source flask-env/bin/activate
     python3 -m flask --version
     ```
   - Ensure the `hello.py` file is present in `/home/azureuser`.

3. **Port Conflicts**:
   - Ensure no other service is running on port 80:
     ```bash
     sudo netstat -tuln | grep :80
     ```

4. **Debugging Logs**:
   - Monitor app logs for errors:
     ```bash
     tail -f ~/flask-app.log
     ```

---

## **Security Recommendations**
- **Do not hardcode credentials**: Always store sensitive information like SSH keys in GitHub Secrets.
- **Use secure SSH configurations**: Only allow authorized keys on the VM.
- **Update dependencies**: Regularly update Python, Flask, and related packages to mitigate vulnerabilities.

