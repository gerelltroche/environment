# macOS Development Environment Setup with Ansible

This Ansible playbook configures a macOS machine for development, focusing on .NET and React workflows. It installs essential tools using Homebrew, ensuring an idempotent setup.

## Prerequisites

1.  **macOS:** This playbook is designed for macOS.
2.  **Homebrew:** The playbook attempts to install Homebrew if it's not found in `/opt/homebrew/bin` (Apple Silicon default) or `/usr/local/bin` (Intel default - uncomment the variable in `setup_macbook.yml` if needed).
3.  **Ansible:** You need Ansible installed on the machine you're running the playbook *from* (which is the target machine in this case). Install it via Homebrew:
    ```bash
    brew install ansible
    ```
    Or via pip:
    ```bash
    pip3 install ansible
    ```

## Included Roles & Software

*   **common:** Installs Homebrew (if needed) and Git.
*   **zsh:** Installs Oh My Zsh and sets the theme to 'minimal'.
*   **dotnet:** Installs the latest stable .NET SDK.
*   **nodejs:** Installs the latest LTS Node.js version (includes npm) and Bun.
*   **vscode:** Installs Visual Studio Code and essential extensions (C# Dev Kit, ESLint, Prettier, AWS Toolkit).
*   **docker:** Installs Docker Desktop.
*   **aws:** Installs AWS CLI v2 and the Session Manager Plugin.
*   **dev_utils:** Installs common development utilities (Google Chrome, iTerm2, Insomnia, Terraform).

## Usage

Navigate to the playbook directory (`c:\Projects\ansible` or wherever you cloned it) in your terminal and run:

```bash
# Run the entire playbook
ansible-playbook setup_macbook.yml
```

### Using Tags

Tags have been added to tasks to allow running specific parts of the setup:

*   **Install only .NET SDK:** `ansible-playbook setup_macbook.yml --tags dotnet`
*   **Install only Node.js:** `ansible-playbook setup_macbook.yml --tags nodejs`
*   **Install only Node.js (LTS) & npm:** `ansible-playbook setup_macbook.yml --tags node`
*   **Install only Bun:** `ansible-playbook setup_macbook.yml --tags bun`
*   **Install only Oh My Zsh & theme:** `ansible-playbook setup_macbook.yml --tags zsh`
*   **Install only VS Code and extensions:** `ansible-playbook setup_macbook.yml --tags vscode`
*   **Install only Docker:** `ansible-playbook setup_macbook.yml --tags docker`
*   **Install only AWS tools (CLI, SSM plugin):** `ansible-playbook setup_macbook.yml --tags aws`
*   **Install only Dev Utilities (Chrome, iTerm2, Insomnia, Terraform):** `ansible-playbook setup_macbook.yml --tags dev_utils`
*   **Install only Terraform:** `ansible-playbook setup_macbook.yml --tags terraform`
*   **Install base tools (Homebrew, Git):** `ansible-playbook setup_macbook.yml --tags common`
*   **Install development editors/tools (VS Code, Docker, AWS):** `ansible-playbook setup_macbook.yml --tags editor,container,aws`
*   **Skip Docker installation:** `ansible-playbook setup_macbook.yml --skip-tags docker`
*   **Skip AWS tools installation:** `ansible-playbook setup_macbook.yml --skip-tags aws`
*   **Skip Zsh/Oh My Zsh setup:** `ansible-playbook setup_macbook.yml --skip-tags zsh`
*   **Skip Dev Utilities installation:** `ansible-playbook setup_macbook.yml --skip-tags dev_utils`

## Best Practices & Recommendations

### Ansible Playbook

*   **Idempotency:** The playbook uses Ansible's built-in modules where possible to ensure tasks are idempotent.
*   **Roles:** Software setup is organized into roles for modularity.
*   **Linting:** Check the playbook for potential issues and best practices using `ansible-lint`:
    ```bash
    # Install if needed: brew install ansible-lint
    ansible-lint .
    ```
*   **Version Control:** Store this playbook directory in a Git repository to track changes:
    ```bash
    git init
    git add .
    git commit -m "Initial commit of Ansible macOS setup playbook"
    # Add a remote origin (e.g., GitHub)
    ```

### Development Tools

*   **Git:** Configure your global Git settings:
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "your.email@example.com"
    ```
    Consider using SSH keys for authentication.
*   **AWS CLI:** After installation, configure your credentials using `aws configure` or other methods.
*   **Node.js:** For managing multiple Node versions, consider installing and using `nvm` (`brew install nvm`). Always commit `package-lock.json` or `yarn.lock` files. Use `npx` for running package commands without global installs. Use ESLint/Prettier. Explore Bun's capabilities (runtime, bundler, package manager).
*   **.NET:** Use `global.json` in your projects to pin SDK versions. Get familiar with the `dotnet` CLI.
*   **VS Code:** Utilize Settings Sync. Consider project-specific settings (`.vscode/settings.json`) and recommended extensions (`.vscode/extensions.json`). Check out the features of the AWS Toolkit extension.
*   **Docker:** Use `.dockerignore` files. Use Docker Compose for local multi-container applications.
*   **Terraform:** Consider using Terraform for managing your AWS infrastructure as code.
*   **Other Tools:** Explore iTerm2 for terminal enhancements and Insomnia for API testing.
