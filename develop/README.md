# Mufasa Development Container

This container provides an environment for developing and testing the MUFASA project, with built-in tools for editing, Git management, and Jupyter Notebook access. It supports pre-saved Git credentials and allows dynamic configuration.

## Features
1. Editable MUFASA Installation:
   - The `mufasa` repository is installed in editable mode, so changes to the source code are immediately reflected.

2. Git Credential Management:
   - The container checks for pre-saved Git credentials and automatically configures Git.
   - If no credentials are found, it prompts the user to enter their Git username, email, and credentials.

3. Pre-installed Tools:
   - Sublime Text for editing code.
   - Jupyter Notebook for interactive workflows.

## Workflow

### 1. Pre-Saved Git Credentials
If you have a saved `.git-credentials` file, place it in the following location before starting the container:
`/home/user/.git-credentials`

Example `.git-credentials` format:
`https://<username>:<password>@github.com`

### 2. Starting the Container
Launch the container through the CANFAR science portal or another container platform. When the container starts:
- If pre-saved Git credentials are found, they will be automatically loaded.
- If no credentials are found, you will be prompted to enter:
  - Git username
  - Git email
  - Your Git credentials (for storage)

### 3. Accessing the Environment
The container starts in the `mufasa` directory:
`/home/user/mufasa`

Use Sublime Text or Jupyter Notebook for development:
- Jupyter Notebook is available at `http://<container-ip>:8888`.
- The Dask dashboard is available at `http://<container-ip>:8787` if running Dask.

## Using Git Inside the Container

### Switching Branches
1. Open a terminal in the container.
2. Navigate to the `mufasa` directory:
    ```
    cd /home/user/mufasa
    ```
3. Check available branches:
    ```
    git branch -a
    ```
4. Switch to a branch:
    ```
    git checkout <branch-name>
    ```
5. Pull the latest changes:
    ```
    git pull origin <branch-name>
    ```

### Making Changes
Edit code using Sublime Text:
    ```
    subl /home/user/mufasa
    ```
Test changes directly in the container.

### Committing Changes
1. Stage your changes:
    ```
    git add .
    ```
2. Commit with a message:
    ```
    git commit -m "Your commit message"
    ```
3. Push your changes:
    ```
    git push origin <branch-name>
    ```
   
## Dynamic Git Configuration
If no pre-saved credentials are provided, the container will prompt you to enter:
1. Git Username
2. Git Email
3. Git Credentials

These settings will be stored for the duration of the container's lifetime.

## Customizing the Container
To customize the container for specific workflows:
1. Clone the repository containing the Dockerfile.
2. Modify the Dockerfile to include additional dependencies or tools.
3. Build the container:
    ```
    docker build -t mufasa-dev .
    ```
4. Deploy it to CANFAR or your local environment.

## Support
If you encounter any issues, please contact your system administrator or the project maintainers.
