# TOML schema reference

## People

Every member of a Rust team is represented by a file in the `people` directory.
The file structure is this:

```toml
name = "John Doe"  # Real name of the person (required)
github = "johndoe"  # GitHub username of the person (required)
github-id = 123456  # GitHub ID of the person (required)
zulip-id = 123456   # Zulip ID of the person (optional)
# You can also set `email = false` to explicitly disable the email for the user.
# This will, for example, avoid adding the person to the mailing lists.
email = "john@doe.com"  # Email address used for mailing lists (optional)
irc-nickname = "jdoe"  # Nickname of the person on IRC, if different than the GitHub one (optional)

[permissions]
# Optional, see the permissions documentation
```

The file must be named the same as the GitHub username.

## Teams

Each Rust team or working group is represented by a file in the `teams`
directory. The structure of the file is this:

```toml
name = "overlords"  # Name of the team, used for GitHub (required)
subteam-of = "gods"  # Name of the parent team of this team (optional)

# The kind of the team (optional). Could be be:
# - team (default)
# - working-group
# - project-group
# - marker-team
kind = "working-group"

[people]
# Leads of the team, can be more than one and must be members of the team.
# Required, but it can be empty
leads = ["bors"]
# Members of the team, can be empty
members = [
    "bors",
    "rust-highfive",
    "rfcbot",
    "craterbot",
    "rust-timer",
]
# Past members of the team. They will not be considered as part of the team,
# but they will be recognized on the website.
alumni = [
    "buildbot",
]
# Optional, include all members of all other teams.
# Defaults to false.
include-all-team-members = false

[permissions]
# Optional, applies to all team members. See the permissions documentation

[leads-permissions]
# Optional, applies only to team leads. See the permissions documentation

# Configure the GitHub integration
# This is optional, and if missing the team won't be synchronized with GitHub
[github]
team-name = "overlords-team"  # The name of the GitHub team (optional)
orgs = ["rust-lang"]  # Organizations to create the team in (required)
# Include members of these Rust teams in this GitHub team (optional)
extra-teams = ["bots-nursery"]

# Define the mailing lists used by the team
# It's optional, and there can be more than one
[[lists]]
# The email address of the list (required)
address = "overlords@rust-lang.org"
# Access level of the list (required)
# - readonly: only users authenticated with Mailgun can send mails
# - members: only members of the list can send mails
# - everyone: everyone can send mails
access-level = "everyone"
# This can be set to false to avoid including all the team members in the list
# It's useful if you want to create the list with a different set of members
# It's optional, and the default is `true`.
include-team-members = true
# Include the following extra people in the mailing list. Their email address
# will be fetched from theirs TOML in people/ (optional).
extra-people = [
    "alexcrichton",
]
# Include the following email addresses in the mailing list (optional).
extra-emails = [
    "noreply@rust-lang.org",
]
# Include all the memebrs of the following teams in the mailing list
# (optional).
extra-teams = [
    "bots-nursery",
]
```

## Permissions

Permissions can be applied either to a single person or to a whole team, and
they grant access to some pieces of rust-lang tooling. The following
permissions are available:

```toml
[permissions]
# All permissions are optional, including the `permissions` section

# Grants access to the @rust-timer GitHub bot
perf = true
# Grants access to the @craterbot GitHub bot
crater = true
# Grants `@bors r+` rights in the repo `rust-lang/some-repo`
bors.some-repo.review = true
# Grants `@bors try` rights in the repo `rust-lang/some-repo`.
# This is a subset of `bors.some-repo.review`, so this shouldn't
# be set if `review` is also set.
bors.some-repo.try = true
```
