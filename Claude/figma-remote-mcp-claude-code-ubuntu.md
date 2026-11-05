# Configure Figma Remote MCP with Claude Code on Ubuntu 24.04

This guide assumes **Claude Code is already installed** on Ubuntu 24.04.
It configures Figma's hosted Remote MCP server, so you do not need the
unofficial Figma Linux desktop application.

## 1. Check that Claude Code is installed

Open a terminal and run:

``` bash
claude --version
```

If the command is not found, restart your terminal and check that the
Claude Code installer added its binary directory to your `PATH`.

## 2. Add Figma Remote MCP to Claude Code

To make the Figma connection available across your Claude Code projects,
run:

``` bash
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

This registers the remote HTTP MCP server under the name `figma` for
your user account.

If you only want the server in the current project, omit `--scope user`:

``` bash
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

## 3. Check the MCP server configuration

Run:

``` bash
claude mcp list
```

You should see `figma` in the list of configured MCP servers. This
confirms that Claude Code has the server configuration; authentication
may still be required.

## 4. Authenticate with Figma

Start Claude Code:

``` bash
claude
```

Inside the Claude Code interactive session:

1.  Enter `/mcp`.
2.  Select the `figma` server.
3.  Choose the authentication option if it is shown.
4.  Complete the browser-based sign-in and authorization flow.
5.  Return to Claude Code.

The exact prompts may vary by Claude Code version. If the browser does
not open automatically, follow the URL or instructions shown in the
terminal.

## 5. Verify the connection

Inside Claude Code, enter:

``` text
/mcp
```

Check that the Figma server is shown as connected.

You can also run this from a separate terminal:

``` bash
claude mcp list
```

Note that the list command shows configured servers; use `/mcp` in the
interactive session to check connection and authentication status.

## 6. Test Figma with a design file

1.  Open the required design in Figma.
2.  Copy the Figma file URL.
3.  In Claude Code, provide a prompt like the following, replacing the
    placeholder with your URL:

``` text
Use the Figma MCP server to inspect this Figma design.

Analyze its frames, components, colors, typography, spacing,
and layout. Summarize the design structure and explain how
to implement it using React and Tailwind CSS.

Figma URL: YOUR_FIGMA_FILE_URL
```

If Claude Code can retrieve the design context, the MCP connection is
working. Access to a particular file still depends on the Figma
account's permissions and the authorization granted.

## 7. Useful management commands

List configured MCP servers:

``` bash
claude mcp list
```

Remove the Figma server configuration:

``` bash
claude mcp remove figma
```

Re-add it at user scope:

``` bash
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

## Troubleshooting

### Figma does not appear in the MCP list

Run the add command again and check for any error:

``` bash
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
claude mcp list
```

If a server named `figma` already exists, remove it before re-adding it:

``` bash
claude mcp remove figma
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

### Authentication does not complete

-   Make sure your browser is signed in to the intended Figma account.
-   Complete the authorization prompt and return to Claude Code.
-   Run `/mcp` again to inspect the server status.
-   If necessary, remove and re-add the server, then repeat
    authentication.

### Claude Code cannot access a design

-   Confirm that the signed-in Figma account has permission to view the
    file.
-   Check that the URL is a Figma design or file URL.
-   Retry with a file that the account can open in the browser.

## Official documentation

-   [Figma MCP Server
    documentation](https://developers.figma.com/docs/figma-mcp-server/)
-   [Figma Remote MCP Server
    installation](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/)
-   [Claude Code MCP
    documentation](https://docs.anthropic.com/en/docs/claude-code/mcp)

------------------------------------------------------------------------

**Note:** This guide configures Figma's **Remote MCP server** at
`https://mcp.figma.com/mcp`. It is different from Figma's local desktop
MCP server, which requires the official Figma desktop application.
Chrome app mode on Linux does not provide that local desktop server.
