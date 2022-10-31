- https://raihan.hashnode.dev/fixup-your-git-history-with-git-fixup

Lets say, we are working on a feature, and you know, when we develop a feature, at first, things are messy, to say the least ;). So we added a bunch of commits. Now, your git log looks something similar to this.

```git
* a81644b780 - oops! fix typo in the service file 
* 59046ea80b - lint and feature done!
* a130b381fd - init service method for the feature
* ca3764b5d8 - create api for the feature
```

Now, if you are like me, who want to leave things in a tidy manner, might feel a bit discomfort to see the last commit. Ideally the change made in the last commit, should have been inside the commit `a130b381fd` . But, it's parent commit `59046ea80b` 's diff probably a huge one, and you are probably hesitating to undo all those changes. It's fine. We can still do this. `git fixup` with interactive rebase and `autosquash` come to the rescue.

The idea of `git fixup` is exactly what it means, to fixup some previous commit. We just need to grab the `SHA` of the commit that needs fixing.

We can get that from `git log` and the copy the specific `SHA`

Then we soft reset the last change. soft reset will preserve whatever change you made.

`git reset --soft HEAD~1`

Now, the last change is in staged area. We can commit, but this time we will do a little bit differently. We will fixup a previous commit. In our case, `a130b381fd`

`git commit --fixup a130b381fd`


Now, run the `git log` to see the change we made.

```git
* 7b96ad9afa - fixup! init service method for the feature 
* 59046ea80b - lint and feature done!
* a130b381fd - init service method for the feature
* ca3764b5d8 - create api for the feature
```

See, we no longer have the unwanted commit? and instead we have a `fixup!` commit, which shadows the commit we want to fix? 

We are half way there, we just need to merge the commit and its fixup. This can be done with the help of interactive rebase and autosquash.

`git rebase -i --autosquash master`

Now, run `git log`

```git
* 59046ea80b - lint and feature done!
* a130b381fd - init service method for the feature
* ca3764b5d8 - create api for the feature
```

See, we have no longer the irritating commit? Thus, git fixup can help us to fix the commit, not just the latest commit with amend. This can be incredibly helpful when we are developing a big feature and want to leave the git history in a meaningful state.