# git-hacks

## .gitconfig

``` 
[user]
    name = Janos Gyerik
    email = info@janosgyerik.com

[alias]
    # aliases in Bazaar
    st = status
    ci = commit
    co = checkout
    # merge for code review
    rev = merge --no-ff --no-commit
    # other custom shortcuts
    br = branch
    ls = ls-files
    lol = log --graph --decorate --oneline
    lola = log --graph --decorate --oneline --all
    revlog = log --first-parent
    untracked = ls-files -o
    add = add -v
    grep = grep -n

    # deployment
    deploy-beta = push releases master:beta
    deploy-prod = push releases master:prod

    # View the SHA, description, and history graph of the latest 20 commits
    l = log --pretty=oneline -n 20 --graph
    # View the current working tree status using the short format
    s = status -sb
    # Show verbose output about tags, branches or remotes
    tags = tag -l
    branches = branch -a
    remotes = remote -v
    # Credit an author on the latest commit
    credit = "!f() { git commit --amend --author \"$1 <$2>\" -C HEAD; }; f"
    today = !git log --since=midnight --author=\"$(git config user.name)\" --oneline
    mr = push -u origin HEAD
    mrf = push -u origin HEAD --force-with-lease
    delete-unused-branches = "!f() { git remote prune origin; git branch | xargs git branch -d; }; f"
    missing = "!f() { test $# -gt 1 && { first=$1; second=$2; } || { first=HEAD; second=$1; }; git rev-list $first..$second --oneline | sed -e 1i'\\ ' -e '1iOnly in '$second: -e 's/^/    /'; git rev-list $second..$first --oneline | sed -e 1i'\\ ' -e '1iOnly in '$first: -e 's/^/    /'; }; f"
    resolve = "!f() { git status -s | sed -ne 's/^UU //p' | xargs -r gvim; }; f"
    undelete = "!f() { git checkout $(git rev-list -n 1 HEAD -- \"$1\")^ -- \"$1\"; }; f"
    current-branch = rev-parse --abbrev-ref HEAD
    follow = "!f() { b=$(git rev-parse --abbrev-ref HEAD); git fetch origin $b; git reset --hard origin/$b; }; f"
    fixtolast = "!f() { p() { git status --porcelain | head -n1 | sed -e s/...//; }; path=$(p); git commit --fixup $(git log --format=%H -n1 $path) $path; }; f"
    testpr = "!f() { [ \"$1\" ] || return; pr=$1; git fetch origin pull/$pr/head:pr/ext/$pr; git checkout pr/ext/$pr; }; f"

    # example usage: git pie s/foo/bar/g
    pie = "!f() {                                            \
        git ls-files -z | xargs -0 perl -e '                 \
            my $eval = shift @ARGV;                          \
            foreach my $file (@ARGV) {                       \
                next unless -f $file;                        \
                open my $fd, $file;                          \
                my $content = do { local $/; <$fd>; };       \
                close $fd;                                   \
                my $new = do { $_ = $content; eval $eval; }; \
                if ($content ne $new) {                      \
                    open my $fd, \">\", $file;               \
                    print $fd $_;                            \
                    close $fd;                               \
                }                                            \
            }                                                \
        ' \"$1\"; }; f"

[apply]
    # Detect whitespace errors when applying a patch
    whitespace = fix

[core]
    # Treat spaces before tabs, and all kinds of trailing whitespace as an error
    whitespace = space-before-tab,trailing-space

[color]
    # Use colors in Git commands that are capable of colored output when outputting to the terminal
    ui = auto
[color "branch"]
    current = yellow reverse
    local = yellow
    remote = green
[color "diff"]
    meta = yellow bold
    frag = magenta bold
    old = red bold
    new = green bold
[color "status"]
    added = yellow
    changed = green
    untracked = cyan
[merge]
    # Include summaries of merged commits in newly created merge commit messages
    log = true

# URL shorthands
[url "git@github.com:"]
    insteadOf = "gh:"
[url "https://github.com/"]
    insteadOf = "ghh:"
[url "git@github.com:janosgyerik/"]
    insteadOf = "me:"
[url "git@bitbucket.org:janosgyerik/"]
    insteadOf = "bbme:"
[url "git@github.com:SonarSource/"]
    insteadOf = "sonar:"
[url "git@gist.github.com:"]
    insteadOf = "gst:"
    pushInsteadOf = "gist:"
    pushInsteadOf = "git://gist.github.com/"
[url "git://gist.github.com/"]
    insteadOf = "gist:"

[push]
    default = matching 

```
