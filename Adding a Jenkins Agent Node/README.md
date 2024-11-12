Here’s a more polished and structured version of your README in Markdown format:

---

# Setting Up a Jenkins Agent on Ubuntu

This guide walks you through creating a Jenkins Agent on your Jenkins Master and configuring it on an Ubuntu server.

---

## Step 1: Create the Jenkins Agent on Jenkins Master

1. **Access Manage Jenkins:**
   - From the Jenkins dashboard, click **Manage Jenkins**.

2. **Add a New Node (Agent):**
   - Under **Manage Jenkins**, select **Manage Nodes and Clouds**.
   - Click **New Node**.
   - Enter a name for your agent (e.g., `Ubuntu-Agent`), select **Permanent Agent**, and click **OK**.

3. **Configure the Agent:**
   - **Description**: Optional; you can leave it empty or add a description.
   - **# of Executors**: Set the number of concurrent jobs this agent can handle (e.g., `1`).
   - **Remote Root Directory**: Specify the agent’s home directory where Jenkins will store files (e.g., `/home/jenkins`).
   - **Labels**: Optional; use labels to assign specific jobs to this agent.
   - **Usage**: Set to **Use this node as much as possible** to allow Jenkins to schedule jobs on this agent.
   - **Launch Method**: Select **Launch agent via SSH**.

4. **Enter Agent Details:**
   - **Host**: Enter the IP address or hostname of your Ubuntu server where the agent will run.
   - **Credentials**: Click **Add** next to Credentials, and create a new SSH credential.
     - **Username**: Typically `jenkins` or your server’s username.
     - **Private Key**: Provide the SSH private key for access (or use a password if required).

5. **Save the Configuration**: Once all details are entered, click **Save**.

---

## Step 2: Set Up the Jenkins Agent on the Ubuntu Server

1. **Install SSH on Ubuntu:**
   ```bash
   sudo apt install openssh-server -y
   ```

2. **Verify SSH is Running:**
   ```bash
   sudo systemctl status ssh
   ```

3. **Create a Jenkins User (if needed):**
   ```bash
   sudo adduser jenkins
   sudo usermod -aG sudo jenkins
   ```

4. **Configure SSH Keys (optional but recommended):**
   - On the Jenkins master server, generate SSH keys:
     ```bash
     ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa
     ```
   - Copy the public key to the Ubuntu server:
     ```bash
     ssh-copy-id jenkins@<agent-server-ip>
     ```

---

## Step 3: Launch the Agent on Ubuntu

1. **Connect the Agent**:
   - After completing the configuration on Jenkins, the agent should automatically connect to the Jenkins master.
   - Go to **Manage Nodes** in Jenkins, and verify the new agent appears as online.

2. **Troubleshoot Connection Issues**:
   - If the agent is not connecting, check SSH connectivity and review configurations on both ends.
   - Check the agent logs in Jenkins by clicking the agent name under **Manage Nodes**.

---

## Step 4: Use the Agent in Jenkins Jobs

- **Assign Jobs to the Agent**:
  - In the job configuration, go to **Restrict where this project can be run** and enter the label you assigned to the agent. This ensures specific jobs run on the designated agent.

---

This completes the setup of your Jenkins Agent on Ubuntu. Now you can assign jobs to the agent and enjoy the distributed workload in Jenkins!
