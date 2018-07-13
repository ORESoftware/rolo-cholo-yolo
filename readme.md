

There are 3 projects:

1. rolo
2. cholo
3. yolo  (the directory is called foo, for a reason)


Each project depends on the 2 other project, in a circular manner, of course.

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



### Instructions on how to replicate the demo


1. Clone nlu_example_projects => ```git clone https://github.com/ORESoftware/rolo-cholo-yolo.git```

2. For each project run:

```nlu init```

When it prompts you for a searchRoot, you can enter this line:

`$HOME/<location>/foo:$HOME/<location>/rolo:$HOME/<location>/cholo`

where <location> is the path to where you cloned the repo.