# Migrating from GitHub to Gitopia 

### Gitopia Mirror Action

Gitopia Mirror Action is a GitHub action for mirroring your GitHub repoistories automatically to another remote repository on Gitopia. Branches, tags, and commits can be mirrored.

Gitopia Mirror Action is available in the [GitHub Marketplace](https://github.com/marketplace/actions/gitopia-mirror-action)

### Pre-requisite:

To use the Gitopia Mirror Action:

- You need to have a repository on GitHub
- You must have at least the Maintainer role for the project
- You need to have a Gitopia wallet file

### 1. To get started, create a new repository on Gitopia. For help [Click Here](https://docs.gitopia.com/repository)

![image](https://docs.gitopia.com/assets/images/mirror1-e0e09d1deaf1b0a5930113501e7bed78.png)

### 2. Once successful, you would be redirected to the new repository page.

![image](https://docs.gitopia.com/assets/images/mirror2-dca3a2a86e3109c01df6048544021e75.png)

### 3. Copy the remote URL from the repo page, as suggested in the image below.

![image](https://docs.gitopia.com/assets/images/mirror3-bfbc7ccd264fe07e35e2f47b92f42dc0.png)

Example:  
> gitopia://gitopia130xtlqs7jxggp7emxmhzc00axpy658qwy804ud/hello-world



### 4. Also download the wallet file by clicking on Download wallet as shown in the image.

![image](https://docs.gitopia.com/assets/images/mirror4-071c0d86b8fe84a3732001be1550bcaa.png)


### 5. Navigate to the GitHub repository you want to migrate to Gitopia on your browser.

![image](https://docs.gitopia.com/assets/images/mirror5-58dfc55e009d6ba0ce84601fdc9f28ba.png)


### 6. You need to add Gitopia wallet contents to GitHub secrets called GITOPIA_WALLET. To do this, navigate to the repository Settings and then click on Secrets as shown in the image.

![image](https://docs.gitopia.com/assets/images/mirror6-4d1a551a7a3c6b1080a83dff0a549e1c.png)


### 7. Click on Actions under Secrets. This will help you create Secrets for your repository. Secrets are environment variables that are encrypted.

![image](https://docs.gitopia.com/assets/images/mirror7-45dc45eff2e653c87ff3ae5a11c8230e.png)


### 8. Click on New repository secret.

![image](https://docs.gitopia.com/assets/images/mirror8-7c4e117910b66dd12ac50f1ca60da567.png)

It will take you to the Action Secret creation page.



![image](https://docs.gitopia.com/assets/images/mirror9-85fd2f3ef1c5e2a9c8a1872e70d06d58.png)


### 9. Fill in the details as shown below.



> - **Name:** GITOPIA_WALLET

> - **Value:** Paste the contents for your Gitopia Wallet file here


A typical wallet file content is shown below:
```
{
  "name": "gitopia-wallet",
  "mnemonic": "magic practice outdoor car vacant unveil lamp absurd cliff region grape burst always creek cat ill hand magic scorpion acquire seminar jump pen hand",
  "HDpath": "m/44'/118'/0'/0/",
  "password": "aaaaaaaa",
  "prefix": "gitopia",
  "pathIncrement": 0,
  "accounts": [
    {
      "address": "gitopia130xtlqs7jxggp7emxmhzc00axpy658qwy804ud",
      "pathIncrement": 0
    }
  ]
}
```


### 10. Click on Add Secret.



![image](https://docs.gitopia.com/assets/images/mirror10-a124e963d410fa2f0780268102658092.png)

Your repository now has a Action Secret named GITOPIA_WALLET. This step is crucial for the Gitopia Mirror Action to function.

![image](https://docs.gitopia.com/assets/images/mirror11-6a6b680d51b387fb3796368afa38680e.png)



### 11. Click on Actions to navigate to the GitHub Actions sections of your repository.


![image](https://docs.gitopia.com/assets/images/mirror12-2306e754a81386fe2cb89770d34a7cb6.png)



### 12. Click on set up a workflow yourself.

![image](https://docs.gitopia.com/assets/images/mirror13-bc77b0a7f0f5d9390d3ac3745226d941.png)

> This will help to create a main.yml file in the .github/workflows/ directory of your repository. The directory will be created automatically if it does not exist.



### 13. Change the file name to gitopia-mirror.yml and its contents to:
```
name: Mirror to Gitopia

on:
  push:
    branches:
      - '**'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Push to Gitopia mirror
        uses: gitopia/gitopia-mirror-action@v0.5.0
        with:
          gitopiaWallet: "${{ secrets.GITOPIA_WALLET }}"
          remoteUrl: "gitopia://gitopia10j4ryjjna69hvsrahgvhwjv3dd46tt6xuprguq/gitopia-mirror-action"
          force: false
```


## Input details
- gitopiaWallet: Your wallet file saved as a GitHub secret. Add Gitopia wallet contents to GitHub secrets called GITOPIA_WALLET as shown in the previous steps.

- remoteUrl: Your Gitopia repository remote_url created from https://gitopia.com

- force (optional): true/false, Force push to gitopia remote repository? Default is false, setting this to true will allow the mirror action to overwrite your gitopia remote repository. The changes done directly in your gitopia remote repository will be overwritten.




#### NOTE
- The checkout action that we use above, by default, checks out a shallow copy of your repo without history. fetch-depth: 0 configures it to checkout the branch with history.

- 
```
on:
  push:
    branches:
      - '**'
```

- In the above snippet, push to any branch will trigger the mirror action. You can change ** to a specific branch and only that branch will be used in the mirror action and get pushed to Gitopia.



> Your file, gitopia-mirror.yml should now look like this:

![image](https://docs.gitopia.com/assets/images/mirror15-e79cde84e1bd7fc09b9c851718dd74e9.png)



### 14. Commit the changes to your repository by clicking on Start commit and then Commit new file after filling in the required details.

![imahe](https://docs.gitopia.com/assets/images/mirror16-e82c9dbf7bf3a0d6d980ea14fd8648bf.png)


> Your repository will now have a .github/workflows/ directory with a file gitopia-mirror.yml in it.


![image](https://docs.gitopia.com/assets/images/mirror17-1396732f57ef48c66c887d6df656b547.png)


### 15. Commiting the changes will triger a Github Action as described in the above .yml. In the future, pushing any branch to GitHub will trigger the Github Action.


![image](https://docs.gitopia.com/assets/images/mirror18-cae29f30b3925cca31dfedde13527392.png)


### 16. As you can see in the above image, the code has been pushed to Gitopia. Now, we wait for the GItopia transaction to confirm, and voila - you can see your repository directory and files on Gitopia:


![image](https://docs.gitopia.com/assets/images/mirror19-225ef497b6b8bcccd598670fc2cc4f3a.png)




# DONE
