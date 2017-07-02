# User Flows

## `git storytell`

### Simple User Flow

*Context: The user has just finished a feature with several commits belonging to this particular feature that is supposed to be reviewed.*

1. User launches `git storytell`. They're prompted to give a name to the story.  
`$ What did you do? ...`
1. Is presented with a list of the latest commits on this branch. Default is either **the oldest commit since HEAD belonging to the user themselves** or **the oldest commit just after a branching**.  
`$ What is the first commit belonging to this feature? ...`
1. Is presented with a list of the latest commits on this branch. Default is **HEAD**.  
`$ What is the newest commit belonging to this feature? ...`
1. The user is presented with a list of all files changed in this commit set.  
`$ Which file should your reviewer look at first? [q to quit]`
1. The user is asked whether they want to comment on the file. If `no`, skip the next step.
1. `$EDITOR` is launched with the diff of that file given as means of context. The user can add their own comments on top (or inline?)
1. GOTO 4 while files are left and user has not typed `q`. (`s/first/next/`).
1. The user is presented with their final story.
1. If confirmed, an empty commit is created with the story as its `COMMITMSG`.

## `git storytold`

*Context: The user is assigned a review and pulls the branch*

1. User launches `git storytold`. They're presented a list of recent storys on the branch:  
`$ Pick the story you want to hear`
1. The story summary is presented to the user.
1. The story of the current file is presented to the user.  
`$ Press ENTER to open $FILE in your editor. Press [g] to view the file on GitHub instead.`
1. The file is opened, either in `$EDITOR` or on GitHub in Comment-mode.  
`$ Press ENTER to move on with the story.`
1. Goto 3 with the next file, or quit if no story is left.

## `COMMITMSG` Format

### Structure

```
STORY: $STORY_SUMMARY
$STORY_DESCRIPTION

// for (i in chapters) {
${i+1}. ${chapter[$i].file}
   ${chapter[$i].comment}
// }
```

### Example

```
STORY: A customer can filter by price

The difficult aspect of this was that we need to determine the lower
and upper boundaries of the price slider while new results come in.

1. /src/app/__tests__/Filters.test.js
   There are two tests here, one so that we can assert that already present
   results are filtered, and one so that new results are filtered as well as
   they come in.
2. /src/app/reducer.js
   Adding this to the reducer was very easy as we're using ramda here. I could
   use some input to this, as the overall structure is rather complicated.
   Maybe we can use ES2017 destructuring here instead?
3. /src/app/__tests__/FiltersComponent.test.js
   Here I added the Price Filter to the component. Had to use a *mount*-ed test
   here as we're testing the reducer here as well.
4. /src/app/FiltersComponent.js
   Well, just had to copy here, so no issues here from my side.
```
