
## NLU Demo Project

<br>

There are 3 projects in this repo:

>
> 1. rolo
> 2. cholo
> 3. yolo
>


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

<i> => (Your main/primary project is whatever project is in in your PWD/CWD). </i>

<br>

## Instructions on how to replicate/run the demo


1. ### Clone rolo-cholo-yolo on your local system

```git clone https://github.com/ORESoftware/rolo-cholo-yolo.git```

<br>

2. ### For each project (rolo, cholo, and yolo) run:

```nlu init```

When it prompts you for a `searchRoot`, you can enter this line:

`$HOME/<location>/yolo:$HOME/<location>/rolo:$HOME/<location>/cholo`

where `<location>` is the path to where you cloned the repo. Using environment varables for `searchRoots`
allows you to work on projects with other people, or yourself, if you work on more than one machine.

So as another step, create this env var and add it to bashrc or bash_profile:

```
export my_nlu_search_root="$HOME/<location>";
```

Then in your .nlu.json files, you can use:

```json
{
  "searchRoots":[
    "$my_nlu_search_root"
  ]
}
```

And for each developer/machine that works on rolo-yolo-cholo, they can define `$my_nlu_search_root` to whatever path they wish.
Essentially each machine can have it's own definition for the searchRoot(s) of your dependency tree.

So, by the way, you have should have run something like this:

```bash
( cd rolo && nlu init )
( cd yolo && nlu init )
( cd cholo && nlu init )
```

3. ### Now run the link!

Now, in each .nlu.json file, there should be a array property called "list".
Notice that "async", "lodash" and "ramda" did not make it into list, but "rolo2", "cholo1" and "yolo3" did.
That is because NLU did not find local copies of ramda and lodash, but did find local copies of the other 3.

Now, you can cd into any project and run:

```bash
$ nlu run
```

It will link up the project with the other 2 projects. Trying running `$ nlu run` in all 3 projects.
Visually inspect the node_modules folder for each project, so see the result.

<br>


#### You can use --dry or --dry-run

For reference, you can use `nlu run --dry` to get a visual of how the linkage will happen, without actually doing any writes.


<br>

#### Final note

You may be thinking - rolo-yolo-cholo is a monorepo. It sure is. But the reason why mono-repos are not the answer for simple development, is because some projects
do *not* belong in the mono-repo but still need to be included as a dependency of projects in the monorepo,
and so the question is then - how do we link projects that don't belong in the monorepo with projects in the monorepo?
If you try to answer that question, you will realize we are back to square one.