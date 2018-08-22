
## NLU Demo Project

#### This repo accommpanies the following video:

<link to video>

<br>

Use this demo project to better understand how npm-link-up works.

## Intro

npm-link-up allows you to declaratively link projects together for local development. <br>
npm-link-up is part of multi-pronged attack to make multi-repos easier to manage with NPM.

<b> The current pieces are: </b>

* [npm-link-up (NLU)](https://github.com/ORESoftware/npm-link-up) => links multiple NPM packages together for local development
* [r2g](https://github.com/ORESoftware/r2g) => tests local packages <i>properly</i> before publishing to NPM
* [npp](https://github.com/ORESoftware/npp) => publish multiple packages and sync their semver versions

<br>

<b> There are 3 projects in this git repo: </b>

>
> 1. rolo
> 2. cholo
> 3. yolo
>

<br> 

Each project depends on the 2 other projects, to demonstrate NLU working with circular deps.
NLU works by searching your fs for relevant projects. To restrict the search you use an env variable,
otherwise, NLU will search starting from $HOME, which is slower. So it's better to restrict it,
which is easy to do.


Depending on what your main/primary project is, the tree looks like this:

```
      └─ cholo1
           ├─ yolo3
           │  ├─ cholo1
           │  └─ rolo2
           └─ rolo2
              ├─ cholo1
              └─ yolo3
```

or like this:

```
        └─ rolo2
           ├─ cholo1
           │  ├─ yolo3
           │  └─ rolo2
           └─ yolo3
              ├─ cholo1
              └─ rolo2

```


or like this:

```
        └─ yolo3
           ├─ cholo1
           │  ├─ yolo3
           │  └─ rolo2
           └─ rolo2
              ├─ cholo1
              └─ yolo3

```

<i> => (Your main/primary project is whatever project is in your PWD/CWD). </i>

<br>

#### As you can see from this image, rolo cholo and yolo are each linked to each other:

<img width="400px" align="right" src="https://raw.githubusercontent.com/oresoftware/media/master/namespaces/nlu/nlu-rolo-cholo-yolo.jpg">

<br>

These 3 NPM packages could be in a mono-repo or could be separated into different VCS repos, doesn't really matter.
We are going to link the packages together by way of their respective node_modules folders, using npm-link-up.

<br>

<br>

<br>

## Instructions on how to replicate/run the demo


1. Install npm-link-up (best to have the latest):

>
>```bash
>$ npm i -g npm-link-up
>```
>

<br>

2. Clone rolo-cholo-yolo on your local system

>
>```bash
>$ git clone https://github.com/ORESoftware/rolo-cholo-yolo.git
>```
>

<br>

3. cd into the cloned project, and run `$ pwd`

You should get something like this:

>
>```
>/home/you/projects/rolo-cholo-yolo
>```
>

<br>

4. For each project (rolo, cholo, and yolo) run:

>
>```bash
>$ nlu init
>```
>

When it prompts you for a `searchRoot`, you can enter this line:

>
>```
>/home/you/projects/rolo-cholo-yolo
>```
>

or better yet, enter this:

>
>```
>$HOME/projects/rolo-cholo-yolo
>```
>

Using environment varables for `searchRoots` allows you to work on projects with other people, or yourself, if you work on more than one machine.
So as another step, create this env var and add it to ~/.bashrc or ~/.bash_profile:

>
>```
>export my_nlu_search_root="$HOME/projects/rolo-cholo-yolo";
>```
>

Then in your .nlu.json files, you can use:

>
>```json
>{
>  "searchRoots":[
>    "$my_nlu_search_root"
>  ]
>}
>```
>

And for each developer/machine that works on rolo/yolo/cholo, they can define `$my_nlu_search_root` to whatever path you want.
Essentially each machine can have its own definition for the searchRoot(s) of your dependency tree.

If you want to keep things ever simpler, you can just use relative paths:

>
>```json
>{
>  "searchRoots":[
>    ".."
>  ]
>}
>```
>

If all projects are in the same directory, the above would work. To start searching two directories below, then this is best:

>
>```json
>{
>  "searchRoots":[
>       "../../"
>  ]
>}
>```
>

So, by the way, you have should have run something like this:

>
>```bash
>( cd rolo && nlu init )
>( cd yolo && nlu init )
>( cd cholo && nlu init )
>```
>

<br>

5. Now run the link!

Now, in each `.nlu.json` file, there should be a array property called "list".
Notice that "async", "lodash" and "ramda" did not make it into list, but "rolo2", "cholo1" and "yolo3" did.
That is because NLU did not find local copies of ramda and lodash, but did find local copies of the other 3.

<br>

Now, you can cd into any of the 3 packages and run:

>
>```bash
>$ nlu run
>```
>

It will link up the project with the other 2 projects. Try running `$ nlu run` in all 3 projects.
Visually inspect the node_modules folder for each project, so see the result.

<br>


### You can use --dry or --dry-run

For reference, you can use `nlu run --dry` to get a visual of how the linkage will happen, without actually doing any writes.


<br>

##### Final note

You may be thinking: rolo-yolo-cholo is a monorepo. It sure is. But the reason why mono-repos are not the answer, is because some projects
do *not* belong in the mono-repo but still need to be included as a dependency of projects in the monorepo,
and so the question is then - how do we link projects that don't belong in the monorepo with projects in the monorepo?
If you try to answer that question, you will realize we are back to square one.