# Next LS

[![Discord](https://img.shields.io/badge/Discord-5865F3?style=flat&logo=discord&logoColor=white&link=https://discord.gg/nNDMwTJ8)](https://discord.gg/6XdGnxVA2A)
[![GitHub all releases](https://img.shields.io/github/downloads/elixir-tools/next-ls/total?label=GitHub%20Downloads)](https://github.com/elixir-tools/next-ls/releases)
[![GitHub Discussions](https://img.shields.io/github/discussions/elixir-tools/discussions)](https://github.com/orgs/elixir-tools/discussions)

The language server for Elixir that just works. 😎

Still in heavy development, currently supporting the following features:

- Compiler Diagnostics
- Code Formatting
- Workspace Symbols
- Document Symbols
- Go To Definition
- Workspace Folders
- Find References
- Extensions
  - Credo

## Editor Support

<ul>
<li>Neovim: <a href="https://github.com/elixir-tools/elixir-tools.nvim">elixir-tools.nvim</a></li>
<li>VSCode: <a href="https://github.com/elixir-tools/elixir-tools.vscode">elixir-tools.vscode</a></li>
<li>
<details>
<summary>Emacs</summary>

Using eglot:

```elisp
(require 'eglot)

(add-to-list 'exec-path "path/to/next-ls/bin/")

(with-eval-after-load 'eglot
  (add-to-list 'eglot-server-programs
               `((elixir-ts-mode heex-ts-mode elixir-mode) .
                 ("nextls" "--stdio=true"))))

(add-hook 'elixir-mode-hook 'eglot-ensure)
(add-hook 'elixir-ts-mode-hook 'eglot-ensure)
(add-hook 'heex-ts-mode-hook 'eglot-ensure)
```

</details>
</li>
<li>
<details>
<summary>Helix</summary>

Add the following config to your `~/.config/helix/languages.toml`.

```toml
[[language]]
name = "elixir"
scope = "source.elixir"
language-server = { command = "path/to/next-ls", args = ["--stdio=true"] }
```

</details>
</li>
</ul>

## Installation

The preferred way to use Next LS is through one of the supported editor extensions.

### VSCode
Works with VSCode for Mac and Linux.

1. Ensure VS Code is properly installed on your system
2. Install the `elixir-tools` extension.
3. Open the extension settings and check the box enabling NextLS (it is disabled by default)
4. When prompted, accept the installation of NextLS
5. Reset VSCode.

If the server is not connecting by default, try opening VSCode via command line: `code .`

### Independent Install
   
If you need to install Next LS on it's own, you can download the appropriate executable from our [GitHub Releases](https://github.com/elixir-tools/next-ls/releases).

These executables are created with [Burrito](https://github.com/burrito-elixir/burrito) and are completely standalone, except you'll still need Elixir and OTP installed in order for it to start a runtime for your application code.

## Development

If you are making changes to NextLS and want to test them locally you can run
`bin/start --port 9000` to start the language server (port 9000 is just an
example, you can use any port that you want as long as it is not being used
already).

Then you can configure your editor to connect to NextLS using that port.

[elixir-tools.nvim](https://github.com/elixir-tools/elixir-tools.nvim)

```lua
{
  nextls = {enable = true, port = 9000}
}
```

Visual Studio Code

```json
{
  "elixir-tools.nextls.adapter": "tcp",
  "elixir-tools.nextls.port": 9000
}
```

### Note

Next LS creates an `.elixir-tools` hidden directory in your project, but it will be automatically ignored by `git`.

## Troubleshooting

### 1. Is `epmd` running?

#### Linux/Mac:

```bash
pgrep -fl epmd
```

If `epmd` is not running, you might need to start it or ensure it starts automatically with your system. You can do this by running `epmd -daemon`.

---

### 2. Ensure `glibc` version is at least 2.34

#### Linux:

```bash
ldd --version
```

This will show the `glibc` version at the top. Ensure it's 2.34 or higher.

#### Mac:

macOS doesn't use `glibc`; it uses the Darwin C Library. Hence, this step is not applicable.

#### Windows:

Windows does not use `glibc`. If you're using a subsystem or tool that needs it, ensure it's updated.

---

### 3. If using VS Code, does work when opened via terminal?

If using a third party packager, sometimes it is required to use the `code` shell command to open VSCode.  This
ensures the right environmental variables are present to allow VSCode to properly initialize the NextLS Server.

1. Ensure the code ([mac](https://code.visualstudio.com/docs/setup/mac), [linux](https://code.visualstudio.com/docs/setup/linux)) shell command is installed.
2. Navigate to your project directory
3. Open your project with the command `code .`

___
### 4. Does it work in TCP mode?

#### All Platforms:

Start the language server in TCP mode and connect to it with your editor, as described above.

When using elixir-tools extension on VS Code: 
1. Set `elixir-tools` IO setting to `TCP`
2. Close VSCode.
3. Run the following in terminal: `~/.cache/elixir-tools/nextls/bin/nextls --port 9000`
4. Open VSCode normally.  If it fails, Repeat Troubleshooting Step 3 (Open Code via terminal).


___
### 5. Firewall Interference

Sometimes, the firewall can interfere with the TCP mode. If you face connection issues, you might try turning off the firewall temporarily to see if it resolves the issue.

This is usually a problem on macOS, as you should see a popup asking if `beam` and `epmd` can accept incoming connections (which you should click "yes").

In future version, we plan to sign and notarize the darwin binaries, which should avoid this popups and make this a non-issue.
