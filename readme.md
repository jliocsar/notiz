<div align=center>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset=https://raw.githubusercontent.com/jliocsar/notiz/main/.github/white-hand.png>
  <img alt=Logo src=https://raw.githubusercontent.com/jliocsar/notiz/main/.github/hand.png width=196>
</picture>

# Notiz

[![Bun][bun-badge]][bun-url] [![MongoDB][mongodb-badge]][mongodb-url] [![Civet][civet-badge]][civet-url]

_Easy to use CLI notes manager_

</div>

---

## Description

Notiz is a CLI notes manager.

It outputs notes in markdown format, meaning you can add tables, code snippets with syntax highlight, URLs and much more to notes that are displayed directly in your terminal.

All notes are stored in your own [MongoDB][mongodb-url] instance: _your data is yours_. The easiest way is using a free M0 database from [MongoDB Atlas][mongodb-atlas-url].
This also means you can treat your notes as actual documents/records that will be persisted until you delete them through either your CLI or directly in your database (i.e. DB tools).

**Keep in mind this is a work in progress!**

## Requirements

- [Bun][bun-url] 1.0.0 or above;
- [MongoDB][mongodb-url];
- Git.

## Installation

Simply install the CLI tool from `npm`:

**Can't install globally with Bun yet, since Bun isn't currently running `postinstall` scripts**.

```sh
npm i -g @jliocsar/notiz@latest
```

Create a secret of your choice and set as an env. variable named `NOTIZ_SECRET` in your terminal's rc file (i.e. `.zshrc`, `.bashrc`...):

```sh
export NOTIZ_SECRET='my secret'
```

Then simply authenticate your database through Notiz's CLI itself:

```sh
notiz auth
```

Make sure you input your MongoDB connection URI with your username & password authentication!

Your URI will be stored in a encrypted file in the `~/.notiz` directory, it'll use your `NOTIZ_SECRET` env. variable as a secret for the encryption.

## Development

All contributions are welcome here!

If you want to contribute, simply clone the repository and run the following command from the root folder:

```sh
bun i
```

This will install all dependencies and transpile files from `.civet` to `.civet.tsx`, so you can also run the command from the `notiz` executable.

The main scripts are:

### Running the command

```sh
bun start
```

### Transpile `.civet` files

```sh
bun transpile
```

> **Note**
> The database credentials used in development are the same from your usual Notiz command, so keep that in mind!

## Usage

```
notiz <cmd> [options]

Commands:
  notiz upgrade                     Upgrades the command to its latest version
                                                                    [aliases: u]
  notiz auth                        Updates the database access configuration
                                                                    [aliases: a]
  notiz configure <option> <value>  Configures the CLI options   [aliases: conf]
  notiz search <content>            Search notes by content         [aliases: s]
  notiz preview                     Preview all notes               [aliases: p]
  notiz delete <id...>              Delete note(s) by ID(s)         [aliases: d]
  notiz open [id]                   Opens a note by its ID       [aliases: o, e]
  notiz view [id...]                Views note(s) by ID(s) [aliases: list, v, l]
  notiz create                      Create a note                [aliases: c, n]

Options:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```

> **Important**
> The default editor used by Notiz is configured by the `notiz configure editor` command, or by setting the `VISUAL`/`EDITOR` env. variables.
> If neither are present, Notiz will use [`nvim`][nvim-url] by default.
>
> Check more details about the editor priority [here](https://github.com/jliocsar/notiz/blob/main/lib/const.civet#L14-L19).

### Create

Creates a new note.

The `create` command will open the configured editor to create a new note.
The metadata part of the document will have a `title` field that's **required**, besides the actual note content.

> **Warning**
> Notes cannot be empty! They must have a content to be stored.

Each note can have an `expires at` date, which is a combination of `[value] [unit]`, the unit being any available from the [dayjs](https://day.js.org/docs/en/manipulate/add#list-of-all-available-units) library.

> *Has the aliases* `c` *and* `n`.

```
notiz create

Create a note

Options:
      --help     Show help                                             [boolean]
      --version  Show version number                                   [boolean]
  -e, --expires  Note expiration time                                   [string]
```

### Open

Opens a note by its ID.

Used for updating or just viewing a note.

Provide `"last"`/`"@"` or no IDs to open the last edited note.

> *Has the alias* `o`.

```
notiz open [id]

Opens a note by its ID

Positionals:
  id  Note id ("last"/"@" or none to open the last one)                 [string]

Options:
      --help     Show help                                             [boolean]
      --version  Show version number                                   [boolean]
  -e, --expires  Sets a new note expiration time                        [string]
```

### View

Views note(s) by ID(s).

Used for viewing the actual content of the note.

Provide `"last"`/`"@"` to view the last edited note, or no ID to view all notes.

> *Has the aliases* `v` *and* `l`.

```
notiz view [id...]

Views note(s) by ID(s)

Positionals:
  id  Note id(s) ("last"/"@" to view the last one, none to view all)
                                                           [array] [default: []]

Options:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```

### Delete

Deletes note(s) by ID(s).

Provide `"all"` to delete all notes.

> *Has the alias* `d`.

```
notiz delete <id...>

Delete note(s) by ID(s)

Positionals:
  id  Note id                                   [array] [required] [default: []]

Options:
  --help     Show help                                                 [boolean]
```

### Configure

Configures the CLI options

> *Has the alias* `conf`.

```
notiz configure <option> <value>

Configures the CLI options

Positionals:
  option  Option to configure            [string] [required] [choices: "editor"]
  value   Value to set the option to                         [string] [required]

Options:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```

### Upgrade

Upgrades Notiz to its latest version.

A simple wrapper around the `npm i -g ...` command.

> *Has the alias* `u`.

```
notiz upgrade

Upgrades the command to its latest version

Options:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```

### Auth

Updates the current database credentials used by Notiz.

The credentials are stored on `~/.notiz/database` in a encrypted file.

> *Has the alias* `a`.

```
notiz auth

Updates the database access configuration

Options:
  --help     Show help                                                 [boolean]
```

## TODO

- [ ] Write docs using GH's Wiki;
- [ ] Auto-sync docs after a few mins.

## Contributing

Feel free to contribute to this project with any features you'd like.

There's no contributing guidelines or anything similar at the moment, but there might be in the future.

---

<div align=center>

_This project is **active** and **not funded**_

Feel free to contribute and/or buy me some coffee if you like it!

[![BuyMeACoffee](https://www.buymeacoffee.com/assets/img/custom_images/purple_img.png)](https://www.buymeacoffee.com/jliocsar)

</div>

[bun-badge]: https://img.shields.io/badge/bun-fbf0df?style=flat-square&logo=bun&logoColor=fbf0df&color=14151a
[bun-url]: https://bun.sh/
[mongodb-badge]: https://img.shields.io/badge/mongo-001D2C?style=flat-square&logo=mongodb&logoColor=00FF5B
[mongodb-atlas-url]: https://www.mongodb.com/atlas/database
[mongodb-url]: https://www.mongodb.com/
[civet-badge]: https://img.shields.io/badge/civet-3e63dd?style=flat-square
[civet-url]: https://civet.dev/
[nvim-url]: https://neovim.io/
