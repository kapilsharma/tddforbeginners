# Chapter 1 - TDD Basics

## Introduction

In this workshop, we are going to make a simple command line calculator. Our calculator will have just `add` and `product` functionality. Requirements are pretty simple but main focus is on initial steps of `Test Driven Development`.

During this workshop we will do/learn following:

 - Create a new Git project and commit it on github.
 - Initialize composer project and understand `composer.json` file.
 - Understand composer auto-loading.
 - Use composer to install PHP Unit.
 - While coding, we will also get an introduction of basic refactoring.
 - Code. While coding, we will follow TDD approach, which include
   - Write Test - which will obviously fail.
   - Code - To make test succeed.
   - Test - To ensure we achieved our requirements.
   - Refactor - To make code better.
   - Test - To ensure refactoring did not brake anything.
   - Done - Move to next task/requirement.

## Project

To code, we need some tasks or requirements. Project section describes what are the requirements of the project we are going to make in this chapter.

In this chapter, **we are going to make a simple command line calculator.**

However, as in real projects, we will make it in parts. As in real projects, we have some initial set of requirements, we achieve them and make project live. Once project is live, we ger next set of requirements and start development for next version of application. Here, we will divide our application is tasks. Please assume every task is an application, supposed to go live after development. This means, every task must be complete in itself.

Once we complete a task, we need to start development of next task, which represent new feature of our application. This means, doing new task should not break previous task and this is where, `test driven development` comes into picture. We will write test of previous tasks, say task 1 and 2, and while working on task 3, PHP Unit will ensure Task 1 and 2 are still working.

This workshop is divided into 8 tasks. I'm not going to tell about next task. Obviously task represent future requirements and no one know what future will be? So we will take one task at a time.

## Task 1

We need to make a command line utility to sum zero, one or two numbers. Command should be

```bash
php calculator.php sum
php calculator.php sum 1
php calculator.php sum 2,3
```

and its output must 0, 1, and 5 respectively.

> Please do not consider the scenarios not mentioned in the task statements. Lets not complicate the situations. We will just do what task ask us to do.

## Task 1 solution

Lets code to achieve task 1 requirements. Before we jump to the solution, we have following prerequisite for this task.

 - `PHP 5.5+`: We assume, PHP 5.5 or higher is already installed. If not, you may check `Appendix C: XAMPP` to setup quick PHP development environment. Although a bit time taking but `Appendix D: Vagrant` or `Appendix E: Docker` is recommended way of setting local PHP development environment.
 - `Git and GitHub account`: We need Git installed locally and you must have github account (free). If you are not aware about git and github, `Appendix A: Git and Github basics` will help you.
 - `Composer`: We will also need composer. If you are not aware of composer, `Appendix B: Composer` will help you.

### Development folder

Although not part of task, I want to recommend how to organize your projects. There must be a single folder say `dev` which must contains all of your projects. Within `dev` folder, you may keep all of your project. If you work on too many projects, may create further sub-folder to arrange different type of projects.

`dev` folder may be kept on location like `/home/<yourname>/dev` on Linux/Mac or `D:\dev` on windows.

> This also means you should never keep your projects in `/var/www` folder. That is a bad practice. Create project in dev folder and use virtual host if needed. I specially mentioned this as I saw many developer putting all of their projects in `/var/www` folder.

### Creating project

Go to the path where you want to make project like `/home/<yourname>/dev/learning/phpreboot` and run following commands:

```bash
mkdir tddworkshop
cd tddworkshop
git init
```

> Going forward, in this chapter, I will call this folder (tddworkshop) as project root. If I say run command in project root, it will mean running command in folder we just created (tddworkshop).

Above command will initialize empty git repository. Create a README.md file and make your first commit. Now create a github project, add remote and push.

> This is optional step but I recommend this as if you stuck somewhere, you must have your code online to get help from me or someone else.

If you are not familiar with git or github, please check `Appendix A: Git and Github basics`.

**.gitignore**

Initializing git project will create a folder `.git`. I do not want to commit few files to my git, for example, I'm using PHP Storm, which create a folder `.idea`. Similarly, Netbeans create a folder `nbproject`. These folder are necessary to manage project in IDE but not the part of project and it does not make sense to commit them on git.

To ignore these files, create a new file named `.gitignore` and add file/folder names in that file. For example, my `.gitignore` file looks like:

```
.idea
```

### Initializing composer project

Composer is a tool to manage dependencies of your PHP project. We too have few dependencies on third party libraries; right now PHP unit.

If you are not familiar with composer, you will find `Appendix B: Composer` useful to install composer and understand composer basics.