# pw

`pw` is a command-line password manager written in Rust, using the Keybase Filesystem for storage.
This means that in order to use it, you will need a Keybase account, and will need to have `/keybase` mounted on your filesystem.

On its initial run, it'll query keybase to determine the path to your private keybase directory,
    and will store your passwords in `/keybase/private/[you]/pw.dat`.
Since querying keybase is relatively slow, on its first run it'll write the path to `pw.dat` to a config file, `~/.pwrc`, and just use that path subsequently.
If you would like your passwords stored somewhere else (e.g. `/keybase/private/[you]/.pw/data` or even `/keybase/private/[you],[other]/pw.dat`),
    simply edit `~/.pwrc` after your first run and point it to wherever you like.

Note that if you replace the datastore path with a non-keybase path, `pw` will happily continue along using a non-encrypted datastore.
This is roughly equivalent to having a `passwords.txt` file on your desktop: you probably don't want to do this.

## Usage

### `pw add [<category>] <name>`

Adds a credential to your data-store.
It will prompt you for your username and password.
The category is optional.

```
$ pw add social facebook.com
Creating new credentials named "facebook.com" in category "social"
Username: my@email.com
Password: mYf4ceb00kPass!
Saved.
$
```

### `pw edit <name>`

Edits an existing credential in your data-store.
It will prompt you for each of its attributes.
Leaving an element blank keeps its current value.
Note that this (currently) makes it impossible to overwrite a non-blank value with a blank value.

```
$ pw edit facebook.com
Name [facebook.com]:
Category [social]:
Username [my@email.com]:
Password [mYf4ceb00kPass!]: n3wp4ss41~
Credential edited.
$
```

### `pw delete <name>`

Deletes an existing credential from your data-store.
It will ask for confirmation.

```
$ pw delete facebook.com
Name: facebook.com
Category: social
Username: my@email.com

Are you sure you wish to delete this credential?
y/n [n]: y
Credential deleted.
$
```

### `pw list`

Displays a list of all of your saved credential names, sorted by category.

```
$ pw list
Category: shopping
    amazon.com
    etsy.com
Category: social
    facebook.com
    linkedin.com
    news.ycombinator.com
    reddit.com
    twitter.com
Category: travel
    united.com
    virginamerica.com
Category: web
    cloudflare.com
    digitalocean.com
    linode.com
    members.nearlyfreespeech.net
$
```

### `pw list categories`

Displays a list of categories.
Note that due to its use as a keyword here, credentials cannot be saved into a category called "categories".

```
$ pw list categories
Categories:
    social
    shopping
    travel
    web
$
```

### `pw list <category>`

Displays a list of credential names within the specified category.

```
$ pw list social
Category: social
    facebook.com
    linkedin.com
    news.ycombinator.com
    reddit.com
    twitter.com
$
````

### `pw show <name>`

Displays the username and password of the specified credential.

```
$ pw show facebook.com
facebook.com:
    my@email
    n3wp4ss41~
$
```

### `pw copy <name> (u|p)`

Places a specified credential's username or password onto your clipboard.
Once copied, `pw` will wait for you to hit the enter key, after which your clipboard is cleared.

```
$ pw copy facebook.com p
facebook.com password copied to clipboard.
(press enter to clear)
$
```

## Installation

Make sure you have rust / cargo / etc. installed. Then:
```
git clone git@github.com:ojensen5115/pw.git    # Clone the repo
cd pw                                          # Enter the directory
cargo install                                  # Copy the binary to your local path
```

## TODO

- lock the password file (e.g. `pw lock` / `pw unlock`)
- consider moving away from bundled sqlite --
  unless you have thousands of passwords,
  it's probably more efficient to just grep a flat textfile than do queries on indexes.

## Known Issues

- I'm clearly not using Docopt quite correctly:
  it isn't properly displaying the summary or the options.
  Should be an easy fix once I sort out what I'm doing wrong.
- Code style likely isn't very good -- comments and pointers much appreciated!