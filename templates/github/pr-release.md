**Deploy Target:** Production

> [!TIP]
> This is a release PR. Code has been reviewed and deployed already.
> Approval required only for the version and changes listed below.
>
> <details>
>   <summary><em>Use <code>git merge --ff-only</code> if possible</em></summary>
>   <dl>
>     <dt>Step 1: Clone or update your local repository</dt>
>     <dd><code>git pull origin production</code></dd>
>     <dt>Step 2: Switch to the production branch</dt>
>     <dd><code>git checkout production</code></dd>
>     <dt>Step 3: Merge the dev branch into production</dt>
>     <dd><code>git merge --ff-only dev</code></dd>
>     <dt>Step 4: Push the changes</dt>
>     <dd><code>git push -u origin production</code></dd>
>   </dl>
>   <p>
>    If <code>git merge --ff-only</code> fails because the target branch has intentional
>    changes, first bring those commits into the base branch (merge or cherry-pick) and
>    retry. Only if you must merge both histories immediately, run
>    <code>git merge --no-ff</code>, then back-merge the target branch into base branch
>    and document the reconciliation.
>   </p>
> </details>

## What's Changed

[Use GitHub's "Generate release notes" button - see instructions]

**Full Changelog**: <https://github.com/epimorphics/[REPOSITORY]/compare/vO.L.D...vN.E.W>
