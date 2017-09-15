# git-tute
For Git and Project workflow practice

## Workflows
There are two workflows:

1. Project workflow
2. Git workflow

### Project Workflow

- It is a simple workflow. Start with [Project Kanban Board](https://github.com/victorskl/git-tute/projects) to create Todo note about an idea, task, topic, bug or a new feature.

- Perform some research, reading and experiment. 

- If an idea is viable, convert it into an issue (right click on Note -> convert to issue). 

- And track the issue until it get implementation done. 

- Once done, follow up with Wiki, (if not fit into - main or module level - README.md) for documenting about it; such as operational manual, knowledge base, and/or how to.

- The iteration may be:

    - Deploy
    - Document at Wiki
    - If an issue arise, create an issue ticket.
	- Bring it into Project management Board 
	(top right > click Add cards > add issue to Todo or In Progress)
    - Fix the issue
    - Close the issue with comments
    - Go to step 1

- NOTE: if you happen to create an issue ticket right away (i.e. without going through note), then make sure, associate the issue ticket to the approprieate column in the Kanban board.

### Git Workflow

- Will be using a combinaiton of Git workflow elaborated in [this][1], [this][2] and [this][3]. Read them later for the background.

- There are [two key branches](https://github.com/victorskl/git-tute/branches) 
	1. master
	2. develop

- The `develop` is the default branch.

- Both of these branches are **protected**, i.e. you can not directly push to these branches.

- First, you clone the default branch i.e. `develop` 
	```
	git clone https://github.com/victorskl/git-tute.git
	cd git-tute
	git status
	```

- Second, you create a new topic branch that must derive from **an issue ticket**, i.e. you do not start any coding without any issue ticket! (For issue ticket creation, follow the **Project Workflow** policy as described in **Project Workflow** section.)

- Depends on the type of an issue, you can use the following naming convention.
	- `feature/topic-name` (e.g. `feature/my-wonderful-feature-A`, `feature/project-shortname-ticket-number-1`)
	- `bug/topic-name` (e.g. `bug/my-stupid-typo`)
	- Or check with the team for an agreed prefix namespace usage
	```
	git checkout -b feature/workflow
	git status
	touch myfile.md
	vi myfile.md
	(add some text)
	```

- While working on the topic branch 
	- You may commit your local topic branch to the main (remote origin) repo.
	```
	git status
	(make sure you are on the topic branch)
	git add .
	git commit -m "issue #1: Initial myfile.md commit"
	git push --set-upstream origin feature/workflow
	...
	...
	(working more on myfile.md)
	...
	...
	git commit -m "fix #1: myfile.md completed"
	```
	- Side note: check GitHub help for [closing issues using keywords](https://help.github.com/articles/closing-issues-using-keywords/)

	- After done with the feature, you shall create a [Pull Request](https://github.com/victorskl/git-tute/pulls) against to the `develop` branch from your topic branch.

- When creating a **Pull Request**, add a respective **code reviewer**. At least, **1 reviewer required to merge** into the `develop` branch.

- At this point, fix if there exist any Unit Test or Integration Test failure. Otherwise you as a developer role is completed. The next phase will be carried out by the DevOps.

- Delete the topic branch.

#### DevOps CI/CD

- This part largely follow [this article][1].

- The development (snapshot) release will be created from the `develop` branch. 

- In CI/CD environment, every commit to `develop` branch is deployable; i.e. all the tests must be passed (Unit Test, Integration Test). Therefore, it requires to make sure, every pull requests from a developer to merge into the `develop` branch must pass all the test cases.

- Deploy the development (snapshot) release into the development environment.

- Create a release branch from the (chosen) `develop` branch to prepare for the staging environment.

- Deploy the release branch to the staging environment.

- If UAT pass at staging environment, create a pull request from the release branch to the `master` branch, and merge.

- Create a release tag at the `master` branch. And deploy it to the production environment. Delete release branch.

- Create a hotfix branch if there need to perform a quick fix to the production. Create pull requests and merge back to both `develop` and `master` branches. Delete hotfix branch.


[1]: http://nvie.com/posts/a-successful-git-branching-model/
[2]: https://www.atlassian.com/git/tutorials/comparing-workflows
[3]: https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows