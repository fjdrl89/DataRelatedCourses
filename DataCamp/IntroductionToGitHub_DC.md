# Introduction to GitHub Concepts

## Description

Do you ever struggle to keep track of everything going on in a project? Or confuse GitHub with Git? In this course, you'll learn how to leverage the power of GitHub, become a successful collaborator, and recognize the differences between GitHub and Git.

Building on the topics covered in Introduction to Version Control with Git, this conceptual course enables you to navigate the user interface of GitHub effectively.

You will perform everyday tasks, including creating public and private repositories, creating and modifying files, branches, and issues, assigning tasks, tagging users, reviewing pull requests, and merging branches. You will also discover how to clone and fork repositories and generate private access tokens (PAT).

By the end of this course, you'll be able to take these new skills and apply them to any coding or data project, making you feel on track and in control. Everyone will want to collaborate with you on GitHub!

## Course Notes

### Chapter 1: Introduction to GitHub

#### 1.1 What is GitHub?

- **Main Definition:** GitHub is a **cloud-based hosting service**.
    
- **Purpose:** It is used for users to upload and **track their work**. Usually, this work is code-based.
    
- **Version Control:** This "tracking" of work is formally known as **version control**.
    
- **Meaning of "Cloud-Based":** GitHub provides on-demand resources (like storage space) to its users over the internet. This prevents us from having to keep large project files on our own computers.
    
- **Popularity:** While alternatives like GitLab and BitBucket exist, **GitHub is the most popular** platform of its kind.
    

##### Uses and Benefits of GitHub

- **Storage and Collaboration:** Its main uses are **storing and tracking projects** and, fundamentally, **collaborating** with other people.
    
- **Social Network:** It also functions as a kind of **social network**, allowing users to connect with each other.
    
- **Open-Source:** GitHub hosts a vast number of **open-source projects** (public projects) that anyone can use to learn, practice, or even contribute to by editing them.
    
- **Solo Projects:** It is also very beneficial for individual (solo) projects, as it provides a **complete history of every project stage**.
    

##### The Key Difference: GitHub vs. Git

This is a crucial point for understanding the platform:
- **Git:**
    
    * It is the **version control software** itself.
        
    * It can be used entirely **independently** of GitHub (e.g., locally or with other platforms).
        
- **GitHub:**
    
    - It is a **platform that enhances and facilitates the use of Git**.
        
    - It makes managing projects and collaboration much easier than using Git alone.
        
    - **Dependency:** GitHub is **completely dependent on Git**. You cannot use GitHub without Git.
        

#####  Collaboration and Repositories (Repos)

- **Easing the Workflow:** GitHub allows the entire version control workflow (tracking changes, allowing multiple people to work on the same files) to happen directly on the web platform, instead of having to use Git via the Command Line Interface (CLI).
    
- **Universal Access:** Since the project is stored in the cloud, **any team member can access it**, making collaboration extremely easy.
    
- **Repository (Repo):**
    
    - A project is stored in a "repository" (or "repo").
        
    - A repository contains **all the project's files** (code, data, etc.) and also the history of all past versions of those files (stored in a `.git` file).
        
- **Remote vs. Local Repository:**
    
    - **Remote Repo:** This is the repository stored on the internet—in this case, on GitHub.
        
    - **Local Repo:** This is the copy of the repository that is saved on a user's local computer.

#### 1.2 Setting up a repo

##### How to Create a New Repository

There are two main ways to start from the GitHub interface:

- **Option 1:** Click the **plus sign (+)** in the top-right corner and select **"New repository"**.
    
- **Option 2:** Go to your **"Repositories"** tab and click the green **"New"** button.
    

Both options lead to the "Create a new repository" page. On this page, you can import an existing repo or use a template, but for this course, we are creating one **from scratch**.


##### Initial Setup Steps

When creating a new repository, you must configure several key items:

- **Name:** The first step is to name your repository (e.g., "soccer-analysis").
    
- **Description:** This is optional but highly recommended, especially for collaborative projects, as it explains what the project is about.
    
- **Visibility (Public vs. Private):**
    
    - **Public:** The project will be **visible to anyone** on the internet.
        
    - Important: Even if it's public, you (the owner) **still control who can make changes** (commits) to the project.
        

##### Initializing Essential Files

Before finishing, it is crucial to initialize the repository with fundamental files:

- **`README` file:**
    
    - It is considered **standard practice** to include one in all repositories.
        
    - It acts as a **guide for the project**. It is vital for collaboration as it provides details on what the project is about and how it can be used.
        
- **`.gitignore` file:**
    
    - **Purpose:** It acts like a "block list." It prevents specific files from being saved (committed) into the repository.
        
    - **Common Use:** It is used to ignore files containing **confidential information** (like passwords or API keys) or unnecessary system files.
        
    - **Templates:** GitHub offers pre-configured templates for different languages (the **Python** template is selected in the video).
        
- **License:**
    
    - In this case, **"None"** is selected.
        
    - This means that **default copyright laws apply**.
        

##### Navigating the Created Repo

Once you hit the "Create repository" button, you are taken to the main view of the new repo, which includes several key tabs:

- **"Code" section:**
    
    - This is the main view where all the project's **folders and files** are visible (including the newly created `README` and `.gitignore`).
        
- **"About" section:**
    
    - Located on the right-hand side, this displays the **description** added during creation.
        
    - It can be edited by clicking the gear (⚙️) icon.
        
- **"Issues" tab:**
    
    - This is the place to **track tasks, bugs, or problems** within the project.
        
    - It is also used to **communicate** with others about these items.
        
- **"Pull Requests" (or PRs) tab:**
    
    - **Definition:** A **request to make a change** to the project.
        
    - **Analogy:** Think of it like a "suggestion box."
        
    - **Function:** A PR shows the suggested changes and **compares them to the project's current version**, allowing the owner to review and decide whether to accept (merge) the changes.
        
- **"Settings" tab:**
    
    - This allows you to make various changes to your repo, such as **changing the name** or managing **access permissions**.

#### 1.3 Creating a README

##### How to Edit the README File

- **Location:** The `README.md` file is always displayed at the bottom of the main **"Code"** section of your repository, just below the list of files.
    
- **File Type:** It is a **Markdown file**, which is indicated by the `.md` extension.
    
- **Editing:** To edit the file, scroll to the rendered README view and click the **pencil icon** located in its top-right corner.
    
- **Edit vs. Preview:**
    
    - **Edit:** This tab shows the raw Markdown syntax.
        
    - **Preview:** This tab shows how the formatted content will look. You can toggle between these to check your syntax as you work.
        

##### Markdown Syntax Fundamentals

Markdown is a lightweight syntax used to format plain text.

- **Headings:**
    
    - Use hashtags (`#`) to create headings.
        
    - `#` (one hashtag) is the largest title.
        
    - `######` (six hashtags) is the smallest available heading.
        
- **Text Formatting:**
    
    - **Bold:** Wrap text in two asterisks (`**text**`).
        
    - _Italic:_ Wrap text in one asterisk (`*text*`).
        
- **Hyperlinks (Links):**
    
    - **Syntax:** `[Visible Text](https.your-url.com)`
        
    - The text in the square brackets `[]` becomes the clickable link.
        
    - The URL in the parentheses `()` is the destination.
        
- **Images:**
    
    - **Syntax:** `![Alternative text](image-url.png)`
        
    - This is similar to a link but starts with an **exclamation mark (`!`)**.
        
    - **Alternative Text:** The text in the `[]` is crucial for **accessibility**. It describes the image for screen readers or if the image fails to load.
        
    - **Drag and Drop:** You can simply drag and drop an image file from your computer directly into the text editor, and GitHub will automatically upload it and generate the correct Markdown syntax.
        

##### What Should a README Include?

The README is the **"instruction manual"** for your repository. It's often the first file a person will see, and its purpose is to allow anyone to understand your project, its contents, and how to use it.

- **Essential Items:**
    
    - Project title
        
    - A description of the project
        
    - Technology used (and why you chose it)
        
    - The process used to answer the project's question (and why)
        
    - A table of contents (for larger projects)
        
- **Characteristics of a _Good_ README (Extras):**
    
    - The motivation behind the project (how it came about).
        
    - Any limitations or challenges encountered.
        
    - A recap of the problem the project intends to solve.
        
    - The project's intended use.
        
    - **Credits:** Always include necessary credits if you are sourcing information or code from elsewhere.


### Chapter 2: Working with Repos

#### 2.1 Modifying a repo structure

##### How to Create a New File

- **Action:** Go to the "Code" section of your repo, click the **`Add file`** button, and select **`Create new file`**.
    
- **Naming:** Type the full filename in the top box, making sure to include the **file extension** (e.g., `requirements.txt`).
    
- **Editing:** Add the desired content into the built-in editor.
    
- **Saving:** See the "Committing the File" section below.
    

##### How to Upload an Existing File

- **Action:** If you have a file on your local computer, click the **`Add file`** button and select **`Upload files`**.
    
- **Process:** You can then **drag and drop** the file (or files) directly onto the page.
    
- **Saving:** See the "Committing the File" section below.
    

##### Committing the File (Saving Changes)

Committing is the act of saving your changes to the repository's history.

- **Commit Message:** At the bottom of the page, add a **brief, descriptive message** in the first text box (e.g., "Add requirements file").
    
- **Extended Description (Optional):** You can add more details in the second box, but it's often left blank for simple changes.
    
- **Commit Options:** You have two choices:
    
    1. **Commit directly to the current branch:** This is the most common approach for small, direct changes to your own repo.
        
    2. **Create a new branch for this commit and start a pull request:** This is used in collaborative workflows (covered later).
        
- **Result:** After committing, GitHub returns you to the "Code" section, where your new or updated file will be visible.
    

##### How to Create a New Directory (Folder)

GitHub has a specific trick for creating directories, as it **does not allow empty directories**.

- **Action:** Click **`Add file`** > **`Create new file`**.
    
- **Process:** In the filename box, type the **name of your directory** followed by a **forward slash (`/`)**.
    
    - _Example:_ To create a directory named `EDA`, you would type: `EDA/`
        
- **Add a File:** The forward slash will move you into the new directory. You must _immediately_ create a file inside it.
    
- **Placeholder:** It's common practice to add a placeholder file, like a `README.md`, which you can add details to later.
    
- **Commit:** Save this new file (e.g., with the message "Add EDA directory") to create the directory.
    

##### How to Modify an Existing File

- **Action:** Navigate to and **click on the file** you want to modify (e.g., `requirements.txt`).
    
- **Editing:** Click the **pencil icon** (✏️) in the file view to open the editor.
    
- **Process:** Add or remove content as needed.
    
- **Commit:** Save your changes with a new commit message (e.g., "Add seaborn to requirements").
    

##### How to Delete a File

- **Action:** Navigate to and **click on the file** you want to delete (e.g., `.gitignore`).
    
- **Process:** In the file view, click the **bin/trash can icon** (🗑️) located in the top-right corner (next to the pencil icon).
    
- **Commit:** Confirm the deletion by saving it as a new commit.



#### 2.2 Working with branches



#### 2.3 Repo access



#### 2.4 Personal Access Tokens (PAT)




### Chapter 3: Collaboration with GitHub

#### 3.1 Using other repos



#### 3.2 GitHub Issues



#### 3.3 Pull Requests



#### 3.4 Reviewing Pull Requests





